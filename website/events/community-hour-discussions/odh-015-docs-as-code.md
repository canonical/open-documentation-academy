# Documentation as Code

## Presenter

[Robert Krátký](https://cz.linkedin.com/in/robertkratky)

## Special thanks

This presentation was developed with valuable input and feedback from:

* [Graham Morrison](https://github.com/degville)
* [Artem Konev](https://ie.linkedin.com/in/artemkonev)
* [Bill Wear](https://www.linkedin.com/in/bill-wear-737825192/)
* [Ruth Fuchss](https://www.linkedin.com/in/rfuchss/)
* [Nick Veitch](https://www.linkedin.com/in/nickveitch/)

---

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=AVNfH99KiME" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Introduction

The philosophy of Documentation as Code, more commonly known as _**Docs as Code**_, bridges the gap between software development and documentation by treating documentation with software tools and processes. In this session, Robert Krátký explains how this approach streamlines documentation development by integrating it closely with source code in software projects.


---

## What is *Docs as Code*?

*Docs as Code* is an approach that places documentation on equal footing with software source code in the software development lifecycle.

This approach avoids reliance on a single monolithic system, that is a traditional software development model that uses one code base to perform multiple functions. Instead, *Docs as Code* leverages specialized tools throughout the content creation, review, and publishing process. Often, the choice of tools aligns with the specific software process it addresses.

---

## Workflow

One of the core objectives of the *Docs as Code* approach is to promote using the most suitable tools for each documentation task. Understanding the purpose and selecting the right tools leads to more effective and valuable documentation.

We can further understand this through the typical lifecycle of documentation when it is maintained as code.

| Goals | Tools | Reasoning |
|---|---|---|
| Content Authoring | Text Editors | Use non-binary, human-readable formats |
| Formatting & Styling | Markup Languages | Semantic formatting with stylesheets enables multiple output formats (HTML, PDF) |
| Version Control | Git, SVN, Mercurial | Essential for tracking changes and history |
| Issue Tracking | JIRA, Bugzilla, Mantis | Manage bugs and improvements |
| Testing & Validation | Scripts, Linters, Spell Checkers | Borrow software testing concepts to ensure quality and reusability |
| Publishing | Static Site Generators, CMS | Automated hosting and updates |

### The Positives

#### Automation

Documentation tooling supports automated processes such as change tracking, testing, and publishing through static site generators or content management systems, helping ensure high quality and smooth update.

#### Version Control

Version-controlled documentation enables:

- Parallel updates and easier collaboration.

- Detailed change tracking that clearly links modifications to individual authors.

- Greater transparency, flexibility, and maintainability across the project.

#### Maintenance

Integrating documentation with code streamlines ongoing maintenance, for example:

- Completing functional documentation in step with software development and key milestones such as releases.

- Running peer reviews of documentation alongside code reviews.

- Keeping documentation versions aligned with the corresponding code on each release branch.

#### Tools

Core tooling is typically open source and portable rather than proprietary (for example, instead of Adobe FrameMaker, MadCap, or RoboHelp), which reduces vendor lock-in and makes future tool changes easier.

Because developers are already familiar with these tools—such as IDEs, text editors, and scripting tools—they can contribute to documentation more naturally, strengthening collaboration.

### The Negatives

- Writers may face a steep learning curve adapting to markup languages and distributed tooling.
- Developers might resist added documentation responsibilities.
- Switching markup dialects (e.g., Markdown to reStructuredText) can be cumbersome.
- Finding user-friendly tools for build and integration is often difficult.
- Transitioning workflows may cause temporary release disruptions.

### Practical Tips

- Use portable and familiar editors for collaboration.
- Keep documentation in a version control system for easy tracking.
- Automated tests and checks can help catch discrepancies early, to the point of preventing release of incorrect software by identifying outdated docs.

---

## What else ...

### Upcoming Innovations

- "Diagrams as Code" is emerging to keep images and graphics updated alongside text and code.
