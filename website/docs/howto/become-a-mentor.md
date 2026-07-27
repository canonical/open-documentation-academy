# CODA mentor handbook

A guide for participants interested in creating issues and mentoring contributors in Canonical's Open Documentation Academy (CODA).

## Creating issues

The heart of the CODA initiative is its [list of documentation-related issues](https://github.com/canonical/open-documentation-academy/issues) that participating projects add to, and contributors select from.

You can create an issue even if you or someone in your team are not committed to mentoring the participant – but do try to find a mentor! Issues without one will be picked up by mentors from the Technical Authors (TA) team.

### Scope the issue

Because of the participants' varying expertise levels, it is critical to correctly scope and write the issues to ensure their success.

There is no guarantee that an issue will be completed on a tight schedule. Choose non-urgent, long-standing issues.

We welcome a broad selection of issues, from those that require no prior understanding of a subject, to those that may need more experience or expert knowledge.

### Create the GitHub issue

Create new issues directly as issues on the [CODA repository](https://github.com/canonical/open-documentation-academy/issues). If you are also creating the issue in your project's repository, link that to the issue created in CODA. There are several elements that we recommend to be included:

- Title
- Background
- Task
- Prerequisites (optional)
- What you will learn
- Outcome
- Resources
- Mentor

#### Title

Start with the associated project name followed by a short description:

```
Ubuntu on WSL: Review contribution guidelines for documentation
```

#### Background

Expand on the title, providing clear context that helps users understand the product and problem:

Rockcraft contains native support for four web app frameworks, including Express. The Express framework has a dedicated tutorial to create a simple "Hello, world" app, package its contents into an OCI-compliant image, and use the container image in some simple ways.

After the npm install step, the terminal outputs a message referring to 14 vulnerabilities (6 low, 5 high, 3 critical). The message looks scary, but it is known, expected, and should not affect the tutorial experience. However, for beginners without much experience, this message might introduce uncertainty or fear into their tutorial experience.

That's where you come in!

#### Task

Include the expected steps for the completion of the issue, including the repository where the PR must be opened in (TODO: remove when we have a dashboard) and mention the requirement of signing the [Canonical contributor license agreement](https://canonical.com/legal/contributors) (CLA):

```
Add a new note admonition that appears after the npm install step in the "Create the Express app" section:

- Mention that the terminal outputs a message referring to vulnerabilities (possibly including the exact text 14 vulnerabilities (6 low, 5 high, 3 critical))
- Briefly describe why the message occurs (npm runs a quick security scan on the downloaded packages and outputs a warning about known security flaws in those packages)
- Reassure the user that this message is expected for the tutorial, the vulnerabilities should not affect the rest of the tutorial, and that the user can safely ignore this warning and proceed as normal.
- Potentially mention that, for production environments, it's best practice to understand and fix the vulnerabilities.

Once you're assigned this issue, please open a pull request directly into canonical/rockcraft. If it's your first time contributing to a Canonical project, you will need to sign the Canonical Contributor License Agreement (CLA).
```

#### Prerequisites

This section is optional. Explicitly state if specific domain knowledge or tools are required, or if the issue is beginner-friendly:

```
This is an ideal issue for anyone with some knowledge of npm and Sphinx, who is willing to help add a note admonition into a preexisting tutorial to offer context and reassurance to the user.
```

#### What you will learn

Briefly list what the contributor will learn from working on this issue:

```
Upon completion of this issue, you will have learned the following:

- Reading error log outputs
- Fixing broken links in open-source documentation in Markdown
- Working with the GitHub pull request workflow
```

#### Outcome

Specify the expected outcome required to close the issue:

```
On completion of this issue, the note admonition will provide additional context that will reassure the user that this warning is expected and can be safely ignored.
```

#### Resources

Include URLs to the current documentation or source files to help the contributor research the problem:

```
- [Rockcraft repository](https://github.com/canonical/rockcraft)
- [Express tutorial](https://documentation.ubuntu.com/rockcraft/latest/tutorial/express/), [source file](https://github.com/canonical/rockcraft/blob/main/docs/tutorial/express.rst)
- [Contributing to the Rockcraft documentation](https://documentation.ubuntu.com/rockcraft/latest/contribute-to-this-documentation/)
- [Rockcraft CONTRIBUTING.md](https://github.com/canonical/rockcraft/blob/main/CONTRIBUTING.md) -- helpful for setting up a development environment

```

#### Mentor

Add who the mentor of the issue is:

```
The mentor for this issue is [GH username]. They are the Technical Author for [Product name].
```

If the mentor is not a TA, the second sentence must be modified accordingly.

If there is no mentor for the issue, include the following:

```
This issue does not currently have a mentor. If you are interested in working on it, please leave a comment and we will assign one as soon as possible. Please bear in mind this might take a few days.
```

Include how to contact the mentor:

```
Please feel free to reach out to the mentor on [Matrix](https://matrix.to/#/@username) if you have further questions.
```

There are several examples of the application of this model: [#352](https://github.com/canonical/open-documentation-academy/issues/352)

### Labels

Make sure to label the issue correctly following the guidelines specified in the CODA repository's [README file](https://github.com/canonical/open-documentation-academy#issue-labels), including the size of the issue.

We use one or more of the following issue labels both for consistency and to indicate what might be expected from a task.

#### [cla](https://github.com/canonical/open-documentation-academy/labels/cla)

Identifies tasks that require a contributor to have signed a [CLA](https://github.com/canonical/open-documentation-academy#contributor-licence-agreement).

#### [code](https://github.com/canonical/open-documentation-academy/labels/code)

Used for tasks that may require some programming knowledge, or a programmatic solution.

#### [diátaxis](https://github.com/canonical/open-documentation-academy/labels/di%C3%A1taxis)

Revise a document to better conform to a [Diátaxis](https://diataxis.fr/) type:

- Tutorial
- How-to
- Reference
- Explanation

This may require a document to be split, edited, or sometimes re-written.

#### [edit](https://github.com/canonical/open-documentation-academy/labels/edit)

Edit pre-existing documentation for consistency, accuracy, style and application.

#### [explanation](https://github.com/canonical/open-documentation-academy/labels/explanation)

Create or revise a document to better reflect an understanding-oriented [explanation](https://diataxis.fr/explanation/).

#### [good first issue](https://github.com/canonical/open-documentation-academy/labels/good%20first%20issue)

An ideal task to start with. Marking issues with this label is a widely adopted [GitHub convention](https://github.com/topics/good-first-issue).

#### [help wanted](https://github.com/canonical/open-documentation-academy/labels/help%20wanted)

Another [GitHub convention](https://github.com/topics/help-wanted) to indicate that a project welcomes community help with an issue.

#### [how-to](https://github.com/canonical/open-documentation-academy/labels/how-to)

Create or revise a document to better reflect a [how-to guide](https://diataxis.fr/how-to-guides/) to achieve a specific goal.

#### [new](https://github.com/canonical/open-documentation-academy/labels/new)

Adding new or missing documentation for a specific tool, feature, or function.

#### [oda-admin](https://github.com/canonical/open-documentation-academy/labels/oda-admin)

Tasks relating to the admin of the Open Documentation Academy (ODA) project.

#### [reference](https://github.com/canonical/open-documentation-academy/labels/reference)

Create or revise a document to better reflect a technical description to use as [reference](https://diataxis.fr/reference/) material.

#### [review](https://github.com/canonical/open-documentation-academy/labels/review)

Review pre-existing documentation for quality, accuracy and consistency. This work may require small updates to the original documentation and/or the creation of sub-tasks to address any detected and substantial shortcomings.

#### [size 1](https://github.com/canonical/open-documentation-academy/labels/size%201) [size 2](https://github.com/canonical/open-documentation-academy/labels/size%202) [size 3](https://github.com/canonical/open-documentation-academy/labels/size%203) [size 5](https://github.com/canonical/open-documentation-academy/labels/size%205) [size 8](https://github.com/canonical/open-documentation-academy/labels/size%208)

This is our estimation of effort and complexity. Size values range from 1 to 8, representing least effort to most effort respectively. These numbers follow the [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_sequence) of 1, 2, 3, 5, 8, with size 8 likely to be a significant undertaking.

#### [ta wanted](https://github.com/canonical/open-documentation-academy/labels/ta%20wanted)

The technical author (TA) team at Canonical wants to help projects without access to documentation experts. This label is used for such projects to mark tasks any technical author can help with.

#### [tutorial](https://github.com/canonical/open-documentation-academy/labels/tutorial)

Develop, write, edit or update a [tutorial](https://diataxis.fr/tutorials/). Tutorials are often the hardest kinds of documentation to write or update because they primarily require good teaching skills and perception, before you even start writing.

#### [update](https://github.com/canonical/open-documentation-academy/labels/update)

Update potentially outdated instructions, commands, or version numbers. These tasks might include release notes, version numbers, new command line arguments and features, and even complete overhauls when a major release occurs.

## Mentoring

CODA aims to be a safe and welcoming environment. Many contributors are working with Git, Markdown, or your specific technology stack for the very first time.

### Communication channels

- GitHub issue: Communication should mainly take place in the GitHub issue to ensure transparency and the existence of one source of truth.
- Documentation Matrix channel: Mentors should join the [main academy documentation channel](https://matrix.to/#/#documentation:ubuntu.com) and interact with the community participants there.

### Provide feedback

We want CODA to be a safe and welcoming environment where everyone who wishes to contribute can make mistakes, improve their skills, learn to collaborate, and ultimately make a valued contribution.

This means, perhaps, that we need to be more patient and descriptive with feedback than we might ordinarily be when responding to pull requests and comments from our own teams.

Try to think like a mentor, or a teacher, and guide contributors towards their objectives. Give them examples and try to nurture their confidence and their curiosity.

Some good examples of these kinds of interactions include the following:

- [Netplan: Add netplan try to Netplan tutorial](https://github.com/canonical/open-documentation-academy/issues/103)
- [Multipass: improve language in the Tutorials](https://github.com/canonical/open-documentation-academy/issues/55)
- [Ubuntu WSL: write alt text for images](https://github.com/canonical/open-documentation-academy/issues/119)

Feedback about the issue in general, such as how to approach it or issues with resources, should be provided in the issue. Specific feedback related to the work done should be provided on the pull request so the team and other people interested have all the details on the specific execution.

### Manage dormant issues and timelines

We aim to be flexible with community timelines, but issue queues must be actively maintained so high-priority or highly sought-after issues do not stall.

#### Standard timelines

- Standard issue duration: Every assigned issue is given a standard timeline of one month. Ask contributors to provide an estimated target date and post updates if they need more time.
- Dormancy period: If an issue sees no activity for several weeks, it is time to intervene. Reach out to them to find out if there is a problem; if they need help e.g. in the form of guidance or more information, or more time. They will be given two weeks to respond.
- Re-release of issues: If they do not respond, give the contributor two more weeks, warning them that we will release the issue to the contribution pool for other interested contributors, or park their original contribution, where applicable, if they don't respond within that time. If they still do not respond, re-release the issue.
- Completion of issue: When an issue is complete, merge the pull request and close the issue on both the project repository and the CODA repository.

Note on priority changes: If an issue unexpectedly becomes high-priority, reach out immediately to the assigned contributor to discuss the status change and mutually agree on whether to fast-track it or reassign it internally.

### Manage undisclosed AI contributions

Our AI guide states all contributions aided by AI should include a disclosure statement. When this is not the case, it might be hard to spot AI usage. There are, however, some signs that could indicate this:

- The [style guide](https://documentation.ubuntu.com/style-guide/) is not followed.
- A PR has been generated immediately after the issue was posted.
- A bot is authoring commits.

The list is not exhaustive. If you spot any of these signs or you feel AI was used, start a conversation with the participant. If they do not comply with the AI guidelines, inform them and close the PR. Make the issue available for another participant.

## Celebrating success and giving credit

Public recognition builds a strong community around Canonical and rewards contributors for their hard work.

### Recognition framework

Helping people to make their first contributions to open source projects, a key objective of the Open Documentation Academy is to recognize the value of such contributions in meaningful ways.

As such, the mentors should recognize significant achievements in the following ways:

- Credit the participant in the project's release notes.
- Provide a certificate (created by the CODA team) that can be shared on social media.

## Additional resources

- [How to create an issue](https://documentation.academy/docs/howto/create-an-issue/)
- [Craft a constructive peer review](https://documentation.academy/library/tutorials/craft-a-constructive-peer-review/)
- [How to implement a peer reviewer's feedback in your documentation](https://documentation.academy/library/reference/how-to-implement-peer-reviewers-feedback/)
