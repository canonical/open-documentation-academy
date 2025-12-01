# Getting hired as a technical writer

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

In the *Docs as Code* approach:

- Documentation tools support automated processes like change tracking, testing, and publishing via static site generators or content management systems ensuring high quality and seamless updates.
  
- Version control of documentation provides:
  - Parallel updates and easier collaboration.
  - Detailed change tracking that clearly matches modifications to their authors.
  - Improved transparency, flexibility, and maintainability across the project.
    
- It streamlines the integration and maintenance of documentation alongside code, for example:
	- Completion of functional documentation hand in hand with software development, synchronized with key project milestones such as software release.
	- Peer document reviews tied closely with code reviews.
	- Accurate document-to-code version alignment, keeping docs synced with the corresponding code on each individual software release branch.
   
- The core tools used are typically open-source and portable, rather than proprietary solutions (e.g., Adobe FrameMaker, MadCap, or RoboHelp). This avoids vendor lock-in, preventing dependency on a single vendor’s ecosystem that complicates switching or migration.
  
- Developers already know these tools- including IDEs, text editors, and scripting tools- making it more natural for them to contribute to documentation, fostering stronger collaboration.

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

### Other Callouts

1. Efforts are underway to streamline collaboration with external projects.
2. Automation is being developed to verify Contributor License Agreement(CLA) before contributors can submit pull requests to the Open Documentation Repository.
3. The Open Documentation schedule is available on the Forum to plan future Office Hour discussion topics.
4. Next week’s session will feature Nick Veitch discussing *Vale* for testing documentation.

