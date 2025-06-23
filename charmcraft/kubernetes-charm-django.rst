.. _write-your-first-kubernetes-charm-for-a-django-app:

Write your first Kubernetes charm for a Django app
==================================================

Imagine you have a Django app backed up by a database such as
PostgreSQL and need to deploy it. In a traditional setup, this can be
quite a challenge, but with Charmcraft you'll find yourself packaging
and deploying your Django app in no time.


Introduction
------------

In this tutorial we will build a Kubernetes charm for a Django
app using Charmcraft, so we can have a Django app up and
running with Juju. Let's get started!

This tutorial should take 90 minutes for you to complete.

If you're new to the charming world, Django apps are
specifically supported with a template to quickly generate a
**rock** and a matching template to generate a **charm**.
A rock is a special kind of OCI-compliant container image, while a
charm is a software operator for cloud operations that use the Juju
orchestration engine. The combined result is a Django app that
can be deployed, configured, scaled, integrated, and so on,
on any Kubernetes cluster.


What you'll need
~~~~~~~~~~~~~~~~

- A local system, e.g., a laptop, with AMD64 or ARM64 architecture
  which has sufficient resources to launch a virtual machine with 4
  CPUs, 4 GB RAM, and a 50 GB disk.
- Familiarity with Linux.


What you'll do
~~~~~~~~~~~~~~

#. Create a Django app.
#. Use that to create a rock with Rockcraft.
#. Use that to create a charm with Charmcraft.
#. Use that to test, deploy, configure, etc., your Django app on a local
   Kubernetes cloud with Juju.
#. Repeat the process, mimicking a real development process.

.. important::

    Should you get stuck or notice issues, please get in touch on
    `Matrix <https://matrix.to/#/#12-factor-charms:ubuntu.com>`_ or
    `Discourse <https://discourse.charmhub.io/>`_


Set things up
-------------

.. include:: /reuse/tutorial/setup_stable.rst
.. |12FactorApp| replace:: Django

Let's create a new directory for this tutorial and enter into it:

.. code-block:: bash

    mkdir django-hello-world
    cd django-hello-world

Finally, install ``python3-venv`` and create a virtual environment:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:create-venv]
    :end-before: [docs:create-venv-end]
    :dedent: 2


Create the Django app
---------------------

Let's start by creating the "Hello, world" Django app that
will be used for this tutorial.

Create a new requirements file with ``nano requirements.txt``.
Then, copy the following text into it, and save:

.. literalinclude:: code/django/requirements.txt
    :caption: ~/django-hello-world/requirements.txt

.. note::

    The ``psycopg2-binary`` package is needed so the Django app can
    connect to PostgreSQL.

Install the packages:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:install-requirements]
    :end-before: [docs:install-requirements-end]
    :dedent: 2

Create a new project using ``django-admin``:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:django-startproject]
    :end-before: [docs:django-startproject-end]
    :dedent: 2


Run the Django app locally
--------------------------

We will test the Django app by visiting the app in a web
browser.

Change into the ``~/django_hello_world`` directory:

.. code-block:: bash

    cd django_hello_world

Open the settings file of the app located at
``~/django_hello_world/settings.py``. Update the ``ALLOWED_HOSTS`` setting
to allow all traffic:

.. code-block:: python

    ALLOWED_HOSTS = ['*']

Save and close the ``settings.py`` file.

Now, run the Django app to verify that it works:

.. code-block:: bash

    ./manage.py runserver 0.0.0.0:8000

.. note::

    Specifying ``0.0.0.0:8000`` allows for traffic outside of the Multipass VM.

    When you run the command for the first time, you will get a warning about unapplied migrations.
    You can ignore this warning for now, as we are currently not performing any database operations. We will set up a database in a later section.

Now we need the private IP address of the Multipass VM. Outside of the
Multipass VM, run:

.. code-block::

    multipass info charm-dev | grep IP


With the Multipass IP address, we can visit the Django app in a web
browser. Open a new tab and visit
``http://<Multipass private IP>:8000``, replacing
``<Multipass private IP>`` with your VM's private IP address.

The Django app should respond in the browser with
``The install worked successfully! Congratulations!``.

The Django app looks good, so we can stop it for now from the
original terminal of the Multipass VM using :kbd:`Ctrl` + :kbd:`C`.


Pack the Django app into a rock
-------------------------------

Now let's create a container image for our Django app. We'll use a rock,
which is an OCI-compliant container image based on Ubuntu.

First, we'll need a ``rockcraft.yaml`` project file. We'll take advantage of a
pre-defined extension in Rockcraft with the ``--profile`` flag that caters
initial rock files for specific web app frameworks. Using the
``django-framework`` profile, Rockcraft automates the creation of
``rockcraft.yaml`` and tailors the file for a Django app. Change
back into the ``~/django-hello-world`` directory and initialize the rock:

.. code-block:: bash

    cd ..
    rockcraft init --profile django-framework

The ``rockcraft.yaml`` file will automatically be created and set the
name based on your working directory, ``~/django-hello-world``.

Check out the contents of ``rockcraft.yaml``:

.. code-block:: bash

    cat rockcraft.yaml

The top of the file should look similar to the following snippet:

.. code-block:: yaml
    :caption: ~/django-hello-world/rockcraft.yaml

    name: django-hello-world
    # see https://documentation.ubuntu.com/rockcraft/en/1.6.0/explanation/bases/
    # for more information about bases and using 'bare' bases for chiselled rocks
    base: ubuntu@22.04 # the base environment for this Django app
    version: '0.1' # just for humans. Semantic versioning is recommended
    summary: A summary of your Django app # 79 char long summary
    description: |
        This is django-hello-world's description. You have a paragraph or two to tell the
        most important story about it. Keep it under 100 words though,
        we live in tweetspace and your description wants to look good in the
        container registries out there.
    # the platforms this rock should be built on and run on.
    # you can check your architecture with `dpkg --print-architecture`
    platforms:
        amd64:
        # arm64:
        # ppc64el:
        # s390x:

Verify that the ``name`` is ``django-hello-world``.

The ``platforms`` key must match the architecture of your host. Check
the architecture of your system:

.. code-block:: bash

    dpkg --print-architecture

Edit the ``platforms`` key in ``rockcraft.yaml`` if required.

Django apps require a database. Django will use a sqlite
database by default. This won't work on Kubernetes because the database
would disappear every time the pod is restarted -- e.g., to perform an
upgrade -- and this database wouldn't be shared by all containers as the
app is scaled. We'll use Juju later to deploy a database.

We'll need to update the ``settings.py`` file to prepare for integrating
the app with a database. Open the file and update the
imports to include ``json``, ``os`` and ``secrets``. The top of the
``settings.py`` file should look similar to the following snippet:

.. code-block:: python
    :emphasize-lines: 15,16,17

    """
    Django settings for django_hello_world project.

    Generated by 'django-admin startproject' using Django 5.1.4.

    For more information on this file, see
    https://docs.djangoproject.com/en/5.1/topics/settings/

    For the full list of settings and their values, see
    https://docs.djangoproject.com/en/5.1/ref/settings/
    """

    from pathlib import Path

    import json
    import os
    import secrets

We need to change some settings to be production ready.
Near the top of the ``settings.py`` file, change the ``SECRET_KEY``,
``DEBUG`` and ``ALLOWED_HOSTS`` variables to:

.. code-block:: python
    :emphasize-lines: 2,5,7

    # SECURITY WARNING: keep the secret key used in production secret!
    SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', secrets.token_hex(32))

    # SECURITY WARNING: don't run with debug turned on in production!
    DEBUG = os.environ.get('DJANGO_DEBUG', 'false') == 'true'

    ALLOWED_HOSTS = json.loads(os.environ.get('DJANGO_ALLOWED_HOSTS', '[]'))

We will also use PostgreSQL as the database for our Django app. In
``settings.py``, go further down to the Database section and change the
``DATABASES`` variable to:

.. code-block:: python
    :emphasize-lines: 3-8

    DATABASES = {
       'default': {
               'ENGINE': 'django.db.backends.postgresql',
               'NAME': os.environ.get('POSTGRESQL_DB_NAME'),
               'USER': os.environ.get('POSTGRESQL_DB_USERNAME'),
               'PASSWORD': os.environ.get('POSTGRESQL_DB_PASSWORD'),
               'HOST': os.environ.get('POSTGRESQL_DB_HOSTNAME'),
               'PORT': os.environ.get('POSTGRESQL_DB_PORT'),
        }
    }

Save and close the ``settings.py`` file. The app will no longer run locally
due to these changes, and we can't test the app until we've deployed
it and connected it to the PostgreSQL database.

Now let's pack the rock:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:pack]
    :end-before: [docs:pack-end]
    :dedent: 2

Depending on your system and network, this step can take several minutes to
finish.

.. admonition:: For more options when packing rocks

    See the :external+rockcraft:ref:`ref_commands_pack` command reference.

Once Rockcraft has finished packing the Django rock, the
terminal will respond with something similar to
``Packed django-hello-world_0.1_<architecture>.rock``. The file name
reflects your system's architecture. After the initial
pack, subsequent rock packings are faster.

The rock needs to be copied to the MicroK8s registry. This registry acts as a
temporary Dockerhub, storing OCI archives so they can be downloaded and
deployed in the Kubernetes cluster. Copy the rock:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:skopeo-copy]
    :end-before: [docs:skopeo-copy-end]
    :dedent: 2

This command contains the following pieces:

- ``--insecure-policy``: adopts a permissive policy that
  removes the need for a dedicated policy file.
- ``--dest-tls-verify=false``: disables the need for HTTPS
  and verify certificates while interacting with the MicroK8s registry.
- ``oci-archive``: specifies the rock we created for our Django app.
- ``docker``: specifies the name of the image in the MicroK8s registry.

.. seealso::

    See more: `Ubuntu manpage | skopeo
    <https://manpages.ubuntu.com/manpages/jammy/man1/skopeo.1.html>`_


Create the charm
----------------

From the ``~/django-hello-world`` directory, create a new directory for
the charm and change inside it:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:create-charm-dir]
    :end-before: [docs:create-charm-dir-end]
    :dedent: 2

Similar to the rock, we'll take advantage of a pre-defined extension in
Charmcraft with the ``--profile`` flag that caters initial charm files for
specific web app frameworks. Using the ``django-framework`` profile, Charmcraft
automates the creation of the files needed for our charm, including a
``charmcraft.yaml`` project file, ``requirements.txt`` and source code for the
charm. The source code contains the logic required to operate the Django app.

Initialize a charm named ``django-hello-world``:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:charm-init]
    :end-before: [docs:charm-init-end]
    :dedent: 2

The files will automatically be created in your working directory.

We will need to integrate our Django app to the PostgreSQL database,
which means we must declare a requirement in the charm project file.
Edit the project file by adding the following section to the end:

.. literalinclude:: code/django/postgres_requires_charmcraft.yaml
    :caption: ~/django-hello-world/charm/charmcraft.yaml
    :language: yaml

.. tip::

    Want to learn more about all the configurations in the
    ``django-framework`` profile? Run ``charmcraft expand-extensions``
    from the ``~/django-hello-world/charm/`` directory.

Now let's pack the charm:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:charm-pack]
    :end-before: [docs:charm-pack-end]
    :dedent: 2

Depending on your system and network, this step can take several
minutes to finish.

Once Charmcraft has finished packing the charm, the terminal will
respond with something similar to
``Packed django-hello-world_ubuntu-22.04-<architecture>.charm``. The file name
reflects your system's architecture. After the initial
pack, subsequent charm packings are faster.

.. admonition:: For more options when packing charms

    See the :literalref:`pack<ref_commands_pack>` command reference.

Deploy the Django app
---------------------

A Juju model is needed to handle Kubernetes resources while deploying
the Django app. The Juju model holds the app along with any supporting
components. In this tutorial, our model will hold the Django app, the
PostgreSQL database, and ingress.

Let's create a new model:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:add-juju-model]
    :end-before: [docs:add-juju-model-end]
    :dedent: 2

Constrain the Juju model to your architecture:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:add-model-constraints]
    :end-before: [docs:add-model-constraints-end]
    :dedent: 2

Now let's use the OCI image we previously uploaded to deploy the Django
app. Deploy using Juju by specifying the OCI image name with the
``--resource`` option:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:deploy-django-app]
    :end-before: [docs:deploy-django-app-end]
    :dedent: 2

Now let's deploy PostgreSQL:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:deploy-postgres]
    :end-before: [docs:deploy-postgres-end]
    :dedent: 2

Integrate PostgreSQL with the Django app:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:integrate-postgres]
    :end-before: [docs:integrate-postgres-end]
    :dedent: 2

It will take a few minutes to deploy the Django app. You can
monitor its progress with:

.. code-block:: bash

    juju status --relations --watch 2s

The ``--relations`` flag will list the currently enabled integrations.
It can take a couple of minutes for the apps to finish the deployment.
During this time, the Django app may enter a ``blocked`` state as it
waits to become integrated with the PostgreSQL database. Due to the
``optional: false`` key in the endpoint definition, the Django app will not
start until the database is ready.

Once the status of the App has gone to ``active``, you can stop watching
using :kbd:`Ctrl` + :kbd:`C`.

.. tip::

    To monitor your deployment, keep a ``juju status`` session active in a
    second terminal.

    See more: :external+juju:ref:`Juju | juju status <command-juju-status>`

The Django app should now be running. We can see the status of
the deployment using ``juju status`` which should be similar to the
following output:

.. terminal::
    :input: juju status

    Model               Controller      Cloud/Region        Version  SLA          Timestamp
    django-hello-world  dev-controller  microk8s/localhost  3.6.2    unsupported  16:47:01+10:00

    App                 Version  Status  Scale  Charm               Channel    Rev  Address         Exposed  Message
    django-hello-world           active      1  django-hello-world               3  10.152.183.126  no
    postgresql-k8s      14.11    active      1  postgresql-k8s      14/stable  281  10.152.183.197  no

    Unit                   Workload  Agent  Address      Ports  Message
    django-hello-world/0*  active    idle   10.1.157.80
    postgresql-k8s/0*      active    idle   10.1.157.78         Primary

To be able to test the deployment, we need to enable debug mode for now.
Set the configuration:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:config-debug]
    :end-before: [docs:config-debug-end]
    :dedent: 2

.. note::

    The ``django-debug`` configuration key sets the ``DJANGO_DEBUG``
    environment variable that we previously updated in ``settings.py``.

    Turning on debug mode shouldn't be done in production. We will do this in
    the tutorial for now and later disable debug mode.

Let's expose the app using ingress. Deploy the
``nginx-ingress-integrator`` charm and integrate it with the Django app:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:deploy-nginx]
    :end-before: [docs:deploy-nginx-end]
    :dedent: 2

The hostname of the app needs to be defined so that it is accessible via
the ingress. We will also set the default route to be the root endpoint:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:config-nginx]
    :end-before: [docs:config-nginx-end]
    :dedent: 2

Monitor ``juju status`` until everything has a status of ``active``.

Now we will visit the Django app in a web browser. Outside of the
Multipass VM, open your machine's ``/etc/hosts`` file in a text editor
and add a line like the following:

.. code-block:: bash

    <Multipass private IP> django-hello-world

Here, replace ``<Multipass private IP>`` with the same Multipass VM
private IP address you previously used.

Now you can open a new tab and visit http://django-hello-world. The
Django app should respond in the browser with
``The install worked successfully! Congratulations!``.


The development cycle
---------------------

So far, we have worked though the entire cycle, from creating an app to
deploying it. But now -- as in every real-world case -- we will go through the
experience of iterating to develop the app, and deploy each iteration.


Add a "Hello, world" app
~~~~~~~~~~~~~~~~~~~~~~~~

In this iteration, we'll add a greeting app that returns a ``Hello, world!`` greeting.

The generated Django project doesn't come with an app, which is why
we had to initially enable debug mode for testing.  We will need to go back
out to the ``~/django-hello-world`` directory where the rock is and enter
into the ``./django_hello_world`` directory where the Django app
is. Let's add a new Django app:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:startapp-greeting]
    :end-before: [docs:startapp-greeting-end]
    :dedent: 2

Open the ``greeting/views.py`` file and replace the content with:

.. literalinclude:: code/django/views_greeting.py
    :language: python

Create the ``greeting/urls.py`` file with the following contents:

.. literalinclude:: code/django/urls_greeting.py
    :language: python

Open the ``django_hello_world/urls.py`` file and edit the imports for
``django.urls`` and the value of ``urlpatterns`` like in the following example:

.. code-block:: python
    :emphasize-lines: 2,5

    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("greeting.urls")),
        path("admin/", admin.site.urls),
    ]


Update the rock
~~~~~~~~~~~~~~~

Since we're changing the app we should update the version of the
rock. Go back to the ``~/django-hello-world`` directory where the rock is
and change the ``version`` in ``rockcraft.yaml`` to ``0.2``. The top of
the ``rockcraft.yaml`` file should look similar to the following:

.. code-block:: yaml
    :caption: ~/django-hello-world/rockcraft.yaml
    :emphasize-lines: 5

    name: django-hello-world
    # see https://documentation.ubuntu.com/rockcraft/en/1.6.0/explanation/bases/
    # for more information about bases and using 'bare' bases for chiselled rocks
    base: ubuntu@22.04 # the base environment for this Django app
    version: '0.2' # just for humans. Semantic versioning is recommended
    summary: A summary of your Django app # 79 char long summary
    description: |
        This is django-hello-world's description. You have a paragraph or two to tell the
        most important story about it. Keep it under 100 words though,
        we live in tweetspace and your description wants to look good in the
        container registries out there.
    # the platforms this rock should be built on and run on.
    # you can check your architecture with `dpkg --print-architecture`
    platforms:
        amd64:
        # arm64:
        # ppc64el:
        # s390x:

Now let's pack and upload the rock using similar commands as before:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:repack-update]
    :end-before: [docs:repack-update-end]
    :dedent: 2


Redeploy the charm
~~~~~~~~~~~~~~~~~~

We'll redeploy the new version with ``juju refresh``.

In the ``./charm`` directory, run:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:refresh-deployment]
    :end-before: [docs:refresh-deployment-end]
    :dedent: 2

Now that we have the greeting app, we can disable debug mode:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:disable-debug-mode]
    :end-before: [docs:disable-debug-mode-end]
    :dedent: 2

Use ``juju status --watch 2s`` again to wait until the App is active
again. You may visit http://django-hello-world from a web browser, or
you can use
``curl http://django-hello-world --resolve django-hello-world:80:127.0.0.1``
inside the Multipass VM. Either way, the Django app should respond
with ``Hello, world!``.


Provide a configuration
-----------------------

To demonstrate how to provide a configuration to the Django app,
we will make the greeting configurable. We will expect this
configuration option to be available in the Django app configuration under the
keyword ``DJANGO_GREETING``. Go back out to the rock
directory ``~/django-hello-world`` using ``cd ..``. From there, open the
``./django_hello_world/greeting/views.py`` file and replace the content
with:

.. literalinclude:: code/django/views_greeting_configuration.py
    :language: python


Update the rock again
~~~~~~~~~~~~~~~~~~~~~

Increment the ``version`` in ``rockcraft.yaml`` to ``0.3`` such that the
top of the ``rockcraft.yaml`` file looks similar to the following:

.. code-block:: yaml
    :caption: ~/django-hello-world/rockcraft.yaml
    :emphasize-lines: 5

    name: django-hello-world
    # see https://documentation.ubuntu.com/rockcraft/en/1.6.0/explanation/bases/
    # for more information about bases and using 'bare' bases for chiselled rocks
    base: ubuntu@22.04 # the base environment for this Django app
    version: '0.3' # just for humans. Semantic versioning is recommended
    summary: A summary of your Django app # 79 char long summary
    description: |
        This is django-hello-world's description. You have a paragraph or two to tell the
        most important story about it. Keep it under 100 words though,
        we live in tweetspace and your description wants to look good in the
        container registries out there.
    # the platforms this rock should be built on and run on.
    # you can check your architecture with `dpkg --print-architecture`
    platforms:
        amd64:
        # arm64:
        # ppc64el:
        # s390x:

Let's pack and upload the rock:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:repack-2nd-update]
    :end-before: [docs:repack-2nd-update-end]
    :dedent: 2


Update the charm
~~~~~~~~~~~~~~~~

Change back into the charm directory using ``cd charm``.

The ``django-framework`` Charmcraft extension supports adding
configurations in ``charmcraft.yaml`` which will be passed as
environment variables to the Django app. Add the following to
the end of the ``charmcraft.yaml`` file:

.. literalinclude:: code/django/greeting_charmcraft.yaml
    :language: yaml

.. note::

    Configuration options are automatically capitalized and ``-`` are
    replaced by ``_``. A ``DJANGO_`` prefix will also be added as a
    namespace for app configurations.

We can now pack a new version of the charm, and then deploy it once more with ``juju
refresh``:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:repack-refresh-2nd-deployment]
    :end-before: [docs:repack-refresh-2nd-deployment-end]
    :dedent: 2

After we wait for a bit monitoring ``juju status`` the app
should go back to ``active`` again. Sending a request to the root
endpoint using
``curl http://django-hello-world --resolve django-hello-world:80:127.0.0.1``
or visiting http://django-hello-world in a web browser should result in the
Django app responding with ``Hello, world!`` again.

Now let's change the greeting:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:change-config]
    :end-before: [docs:change-config-end]
    :dedent: 2

After we wait for a moment for the app to be restarted, using
``curl http://django-hello-world --resolve django-hello-world:80:127.0.0.1``
or visiting http://django-hello-world should now respond with ``Hi!``.


Tear things down
----------------

We've reached the end of this tutorial. We went through the entire
development process, including:

- Creating a Django app
- Deploying the app locally
- Packaging the app using Rockcraft
- Building the app with Ops code using Charmcraft
- Deplyoing the app using Juju
- Integrating the app with PostgreSQL to be production ready
- Exposing the app using an ingress
- Adding an initial app and configuring the app

If you'd like to quickly tear things down, start by exiting the Multipass VM:

.. code-block:: bash

    exit

And then you can proceed with its deletion:

.. code-block:: bash

    multipass delete charm-dev
    multipass purge

If you'd like to manually reset your working environment, you can run the
following in the rock directory ``~/django-hello-world`` for the tutorial:

.. literalinclude:: code/django/task.yaml
    :language: bash
    :start-after: [docs:clean-environment]
    :end-before: [docs:clean-environment-end]
    :dedent: 2

You can also clean up your Multipass instance by exiting and deleting it
using the same commands as above.

Next steps
----------

By the end of this tutorial you will have built a charm and evolved it
in a number of typical ways. But there is a lot more to explore:

.. list-table::
    :widths: 30 30
    :header-rows: 1

    * - If you are wondering...
      - Visit...
    * - "How do I...?"
      - :ref:`How-to guides <how-to-guides>`,
        :external+ops:ref:`Ops | How-to guides <how-to-guides>`
    * - "How do I debug?"
      - `Charm debugging tools <https://juju.is/docs/sdk/debug-a-charm>`_
    * - "How do I get in touch?"
      - `Matrix channel <https://matrix.to/#/#12-factor-charms:ubuntu.com>`_
    * - "What is...?"
      - :ref:`reference`,
        :external+ops:ref:`Ops | Reference <reference>`,
        :external+juju:ref:`Juju | Reference <reference>`
    * - "Why...?", "So what?"
      - :external+ops:ref:`Ops | Explanation <explanation>`,
        :external+juju:ref:`Juju | Explanation <explanation>`