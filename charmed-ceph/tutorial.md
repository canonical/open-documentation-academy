This tutorial will guide you through the process of deploying a Ceph cluster on [LXD](https://canonical.com/lxd) with [Juju](https://juju.is/). You will use Juju to pull the `ceph-mon` and `ceph-osd` charms from [Charmhub](https://charmhub.io/) and colocate them on three LXD machines. To do this, you will need to create a Juju controller on your local LXD cloud to manage your deployment. 

You will then add a model to deploy your charms, and make the services deployed by these charms aware of each other using [Juju integrations](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/juju-cli/list-of-juju-cli-commands/integrate/).

By the end of the tutorial, you will have deployed a standalone Ceph model on LXD with three nodes each containing a `ceph-osd` unit and `ceph-mon` unit, that are integrated with Juju, and you will be ready to customise your Charmed Ceph cluster even more and explore more use cases.

## Prerequisites
 
* A host running Ubuntu 22.04 (Jammy) or later, with:  
  * at least 4 CPU cores  
  * 16 GiB RAM  
  *  90 GiB disk space  
* An internet connection to access [charmhub](https://charmhub.io/)
  * To verify, use `ping charmhub.io` in your terminal.

## Install Juju

First, install Juju as a snap package from the Snap Store.

```
sudo snap install juju
```

## Install and initialise LXD

Install LXD also as a snap from the Snap Store.

```
sudo snap install lxd
```

[note type="positive" status="info"]
If you get an error message that the LXD snap is already installed, run  `sudo snap refresh lxd` to refresh it and ensure that you are running an up-to-date version:
[/note]

Running `lxd init` starts an interactive configuration process on your terminal. Add the `--auto` flag to the command to skip the configuration steps and create a setup with default options.

```
lxd init --auto
```

## Bootstrap a Juju Controller

Next, create a Juju controller to manage your Ceph deployment with the cloud name `localhost` and controller name `lxd-controller.`

This process takes about 5 minutes.

```
juju bootstrap localhost lxd-controller
```

```
Creating Juju controller "lxd-controller" on localhost/localhost
Looking for packaged Juju agent version 3.6.3 for amd64
Located Juju agent version 3.6.3-ubuntu-amd64 at https://streams.canonical.com/juju/tools/agent/3.6.3/juju-3.6.3-linux-amd64.tgz
To configure your system to better support LXD containers, please see: https://documentation.ubuntu.com/lxd/en/latest/explanation/performance_tuning/
Launching controller instance(s) on localhost/localhost...
 - juju-6f74c7-0 (arch=amd64)            	 
Installing Juju agent on bootstrap instance
Waiting for address
Attempting to connect to 10.125.39.190:22
Attempting to connect to [fd42:7fd9:2c4e:1ff8:216:3eff:fe86:5d1a]:22
Connected to 10.125.39.190
Running machine configuration script...
Bootstrap agent now started
Contacting Juju controller at 10.125.39.190 to verify accessibility...

Bootstrap complete, controller "lxd-controller" is now available
Controller machines are in the "controller" model

Now you can run
	juju add-model <model-name>
to create a new model to deploy workloads.
```

Note that the bootstrap process creates a controller machine which will be useful for managing our Juju deployment, and places it in the controller model. 

Run the following command to list all your controllers:

```
juju list-controllers
```

You will notice that a controller named `lxd-controller`, like we had specified, has been created. 

Also note that we don’t have anything under “Model” because we have not created a model yet, but one model is listed under “Models”. This refers to the controller model that was automatically created during the bootstrapping process. 

Your output should look something like this:

```
Use --refresh option with this command to see the latest information.

Controller      Model     User       Access 	 Cloud/Region     	Models  Nodes	HA    Version	   
lxd-controller*   -       admin      superuser   localhost/localhost   1  	  1     none   3.6.3 
```

## Add a Juju model

Create a Juju model to deploy your cluster. We’ll name it `ceph-charms` because we will use it to deploy our Ceph charms, but you can name it anything memorable.

```
juju add-model ceph-charms
```

```
Added 'ceph' model on localhost/localhost with credential 'localhost' for user 'admin'
```

List your models this way:

```
juju list-models
```

The output of the list command will show the model we just created, and the controller model. It also shows the controller on which our model is created. Notice that our model does not have any machines or units  deployed onto it yet.

```
Controller: lxd-controller

Model     	Cloud/Region     	Type  Status 	Machines  Units  Access  Last connection
ceph-charms*  localhost/localhost  lxd   available     	0  	-  admin   never connected
controller	localhost/localhost  lxd   available     	1  	1  admin   just now
```

## Deploy your Ceph cluster

We will deploy three machines and six units on the model, with two units (a `ceph-osd` and `ceph-mon` unit) colocated on each machine.

### Deploy the OSD units

Use Juju to deploy three `ceph-osd` units to your cluster.

```
juju deploy ch:ceph-osd -n 3 --storage osd-devices="loop,5G,1" --constraints "virt-type=virtual-machine root-disk=20G"
```

In this command, Juju pulls the `ceph-osd` charm from Charmhub, creates three virtual machines and deploys the charm on all three machines. The output will look something like this.

```
Deployed "ceph-osd" from charm-hub charm "ceph-osd", revision 622 in channel quincy/stable on ubuntu@20.04/stable
```

[note type="positive" status="info"]
LXD machines are usually containers. However, LXD does not support adding block devices to containers.  We need to add block devices to our `ceph-osd` units, so we will use the  `-- constraints` flag to specify that we want our machines to be virtual machines, which will allow us to do this, rather than containers.
[/note]

The `--storage` flag commissions storage devices to the deployed units.  

`osd-devices` is a storage option that is specific to the `ceph-osd` charm;  it is used to declare block devices that should be enrolled as OSDs automatically. It lists what block devices can be used for OSDs across the cluster. 

Now, let’s run `juju list-models` again:

```
Controller: lxd-controller

Model     	Cloud/Region     	Type  Status 	Machines  Units  Access  Last connection
ceph-charms*  localhost/localhost  lxd   available     	3  	3  admin   15 minutes ago
controller	localhost/localhost  lxd   available     	1  	1  admin   just now
```

And we’ll see that our model now contains three machines and three units.

### Deploy the Monitor units

Remember that three machines had been created by Juju in the previous step. We will now reuse them by collocating `ceph-mon` units with the `ceph-osd` units already on them

To find the names of our deployed machines, run `juju list-machines`.

```
Machine  State	Address    	Inst id    	Base      	AZ  Message
0    	started  10.125.39.192  juju-687c44-0  ubuntu@20.04  	Running
1    	started  10.125.39.193  juju-687c44-1  ubuntu@20.04  	Running
2    	started  10.125.39.191  juju-687c44-2  ubuntu@20.04  	Running
```

The output of this command shows that machines 0, 1 and 2 are running. We will use these machine numbers to make sure that the `ceph-mon` units we will deploy next are colocated on the same machines. 

```
juju deploy -n 3 --to 0,1,2 ceph-mon
```

Unlike Ceph OSD units, Ceph Monitor units don’t need block devices. Therefore, they can be stored in containers. This command will create containers on the three machines for the `ceph-mon` units.

```
Deployed "ceph-mon" from charm-hub charm "ceph-mon", revision 243 in channel quincy/stable on ubuntu@20.04/stable
```

When we run `juju list-models` again, we’ll see that we now have 3 machines and 6 units in our model, with no relations. This means that the deployed units do not know about each other.

```
Controller: lxd-controller

Model    	  Cloud/Region     	  Type   Status  	Machines  Units  Access  Last connection
ceph-charms*  localhost/localhost  lxd   available     3  	    6    admin   30 minutes ago
controller    localhost/localhost  lxd   available     1  	    1    admin   just now
```

### Connect the OSD and Monitor units together

We need the `ceph-osd` and `ceph-mon` services to know about each other. Since the units are already on the same model, it is easy to integrate them using Juju.

```
juju integrate ceph-osd:mon ceph-mon:osd
```

### Monitor the deployment

Check the status of your model, including the machines and units in it . Use the `--watch` flag and specify `2s` to automatically refresh the status of the model after every two seconds.

```
juju status --watch 2s
```

Wait and watch the model stabilise. The output should look something like this:

```
Model  Controller  	Cloud/Region     	Version  SLA      	Timestamp
ceph-charms   lxd-controller  localhost/localhost  3.6.3	unsupported  15:32:25+03:00

App   	Version  Status   	Scale  Charm 	Channel    	Rev  Exposed  Message
ceph-mon       	maintenance	2/3  ceph-mon  quincy/stable  243  no   	installing charm software
ceph-osd       	maintenance	1/3  ceph-osd  quincy/stable  622  no   	installing charm software

Unit     	Workload 	Agent   	Machine  Public address                      	Ports  Message
ceph-mon/0   maintenance  executing   0    	fd42:7fd9:2c4e:1ff8:216:3eff:fe72:20a2     	(install) installing charm software
ceph-mon/1*  waiting  	allocating  1    	fd42:7fd9:2c4e:1ff8:216:3eff:fec9:3809     	agent initialising
ceph-mon/2   maintenance  executing   2    	fd42:7fd9:2c4e:1ff8:216:3eff:fe42:1999     	(install) installing charm software
ceph-osd/0   waiting  	allocating  0    	fd42:7fd9:2c4e:1ff8:216:3eff:fe72:20a2     	agent initialising
ceph-osd/1*  maintenance  executing   1    	fd42:7fd9:2c4e:1ff8:216:3eff:fec9:3809     	(install) installing charm software
ceph-osd/2   waiting  	allocating  2    	fd42:7fd9:2c4e:1ff8:216:3eff:fe42:1999     	agent initialising

Machine  State	Address                             	Inst id    	Base      	AZ  Message
0    	started  fd42:7fd9:2c4e:1ff8:216:3eff:fe72:20a2  juju-cbc3a3-0  ubuntu@20.04  	Running
1    	started  fd42:7fd9:2c4e:1ff8:216:3eff:fec9:3809  juju-cbc3a3-1  ubuntu@20.04  	Running
2    	started  fd42:7fd9:2c4e:1ff8:216:3eff:fe42:1999  juju-cbc3a3-2  ubuntu@20.04  	Running
```

Once stabilised, the units in your model should be in active/idle state, and the machines should all be running. Observe that the Ceph OSD and Monitor units were indeed colocated on machines `0`, `1` and `2`.

```
Model  Controller  	Cloud/Region     	Version  SLA      	Timestamp
ceph   lxd-controller  localhost/localhost  3.6.3	unsupported  15:50:10+03:00

App   	Version  Status  Scale  Charm 	Channel    	Rev  Exposed  Message
ceph-mon  17.2.7   active  	3  ceph-mon  quincy/stable  243  no   	Unit is ready and clustered
ceph-osd  17.2.7   active  	3  ceph-osd  quincy/stable  622  no   	Unit is ready (1 OSD)

Unit     	Workload  Agent  Machine  Public address                      	Ports  Message
ceph-mon/0   active	idle   0    	fd42:7fd9:2c4e:1ff8:216:3eff:fe72:20a2     	Unit is ready and clustered
ceph-mon/1*  active	idle   1    	fd42:7fd9:2c4e:1ff8:216:3eff:fec9:3809     	Unit is ready and clustered
ceph-mon/2   active	idle   2    	fd42:7fd9:2c4e:1ff8:216:3eff:fe42:1999     	Unit is ready and clustered
ceph-osd/0   active	idle   0    	fd42:7fd9:2c4e:1ff8:216:3eff:fe72:20a2     	Unit is ready (1 OSD)
ceph-osd/1*  active	idle   1    	fd42:7fd9:2c4e:1ff8:216:3eff:fec9:3809     	Unit is ready (1 OSD)
ceph-osd/2   active	idle   2    	fd42:7fd9:2c4e:1ff8:216:3eff:fe42:1999     	Unit is ready (1 OSD)

Machine  State	Address                             	Inst id    	Base      	AZ  Message
0    	started  fd42:7fd9:2c4e:1ff8:216:3eff:fe72:20a2  juju-cbc3a3-0  ubuntu@20.04  	Running
1    	started  fd42:7fd9:2c4e:1ff8:216:3eff:fec9:3809  juju-cbc3a3-1  ubuntu@20.04  	Running
2    	started  fd42:7fd9:2c4e:1ff8:216:3eff:fe42:1999  juju-cbc3a3-2  ubuntu@20.04  	Running
```

Congratulations! Your Charmed Ceph cluster is up and running!

## Cleaning up resources

In case, for any reason, you want to get rid of your Ceph cluster,  you can destroy your model it this way:

`juju destroy-model ceph-model --destroy-storage`

This command will terminate all the virtual machines, containers and resources in our model including [applications](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/application/), which consist of our deployed units.  
The `--destroy-storage` flag will get rid of the storage device we had commissioned when deploying our units earlier, using the `--storage` flag.

```
WARNING This command will destroy the "ceph-charms" model and affect the following resources. It cannot be stopped.

 - 3 machines will be destroyed
  - machine list:
 - 2 applications will be removed
  - application list: "ceph-mon" "ceph-osd"
 - 0 filesystem and 3 volumes will be destroyed

To continue, enter the name of the model to be unregistered: ceph-charms
Destroying model
Waiting for model to be removed, 3 machine(s), 2 application(s), 3 volumes......
Waiting for model to be removed.......
Model destroyed.
```

[note type="positive" status="info"]
The above command only terminates machines/containers and resources for the non-controller model. You may create other models on the controller.

If you wish to destroy the controller, too, and clean up all its resources, run `juju destroy-controller lxd-controller`
[/note]

## Next steps

You have deployed a Charmed Ceph model on three LXD machines, colocated `ceph-osd` and `ceph-mon` units on them, and integrated the units with Juju.

See our [how-to guides](https://ubuntu.com/ceph/docs/howtos) to find out how you can manage your cluster, and how to achieve specific goals with Charmed Ceph.

Or, explore our [Explanation](https://ubuntu.com/ceph/docs/explanation) and [Reference](https://ubuntu.com/ceph/docs/reference) sections for additional information and quick references.
