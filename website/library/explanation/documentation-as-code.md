# Documentation as Code: A Comprehensive Study Guide
**Presented by:** [Robert Krátký](https://github.com/rkratky) at Documentation Office Hours - ODH-015

**Link to the video:** [Documentation as Code](https://www.youtube.com/watch?v=AVNfH99KiME)

## What does "Docs as Code" mean?
Is a methodology that treats documentation like software source code, applying similar development practices to create and maintain documentation.
This approach emphasizes using specialized tools for different stages of documentation development.

### Key Characteristics of Documentation as Code
The main difference between **Documentation as Code** and traditional documentation approaches is that instead of using a single monolithic system for the entire process,
it employs specialized tools for each step of the documentation lifecycle.

#### Tooling Components

|  **Goals** | **Tools** | **Reason** |
|----------|----------|----------|
| Content authoring | Text editors | Not binary format, quite easy to start |
| Formatting, styling | Markup languages | The formatting and styling is based in semanticity as oppossed to the formatting being applied directly to the content enabling multiple output formats |
| Version control | Git, SVN, Mercurial, ... | Utilizes distributed version control systems like Git |
| Issue tracking | JIRA, Bugzilla, Mantis... | Provides comprehensive change tracking capabilities |
| Testing, validation | Scripts, linters, spell-checks | Inspired by software testing (like unit tests). When scripts or filters are applied, proper testing ensures reusable checks for content accuracy |  
| Publishing | Site builders, CMS, ... | Uses Site builders to produce web ready HTML or feeds content into CMS systems, mirroring software development workflows |

## Benefits of Documentation as Code

### Developer Perspective
1. **Developer Familiarity & Confort:** Developers already know the tools (Git, scripting,etc) making it easier for them to contribute to documentation.
2. **Higher Engament:** Documentation becomes a natural part of the project, encouraging developers to treat it seriously and make quick, one-off fixes.
3. **Better Integration:** Documentation can be included in the "definition of done" ensuring it's part of product planning and validation, just like code.
4. **Shared Responsability:** QA teams and developers can validate documentation, fostering alignment and reducing the perception of docs as an afterthought.
5. **Open Tooling Capabilities:** Using open source tooling for documentation promotes collaboration and consistency with the software development process.
Documentation integrated into development workflow

### Organizational Benefits
Using open source tooling (like plan text markup and version control) avoids vendor lock-in unlike proprietary tools, which risk obsolescense or abandonment.
This approach also offers flexibility in migrating formats, reduces organizational overhead, and aligns processes across teams,
enabling consistent automation and smoother coordination with software releases.

## Challenges and Considerations

1. **Resistance to change:** Teams used to existing workfkows (especially monolithic documentation tools) may push back due to unfamiliarity with new tools or have steep learning curves.
2. **Developer concerns:** Including documentation in the definition of done might slow down development agility and some developers may dislike increased visibility or accountability for docs.
3. **Technical hurdles:** Migrating legacy documentation to plain-text markup can be non-trivial, and tight schedules may risk release disruptions during the transition.
4. **Balancing Trade-offs:** While the benefits often outweigh these challenges, they must be carefully consideredes to ensure smooth adoption.

## Additional Commentaries

The docs as code approach offers key benefits like enabling documentation review in developers familiar environments (Github/Gitlab) and simpliying change tracking by keeping docs with code,
but also presents challenges such as tying documentation to development cycles and release freezes, difficulty switching between markup languages, significant time spent on formatting issues,
and the need for specialized tools that handle both writing and technical aspects effectively, potentially requiring dedicated staff for markup-related tasks despite its growing popularity.

### Branching and Versioning
Documentation as code my not suit every situation, its strongest advantage is seamless integration with software development branches,
Allowing documentation to evolve alongside code versions (e.g. maintaining separate docs for v1.0 fixes while developing v2.0). Key recommendations include using portable formats,
familiar editors and version control to maintain flexibility This approach is better for collaborative developer documentation but may be less suitable for regulatory and marketing content
requiring centralized technical writer control. The succesfull documentation as code implementation depends on team structure and documentation purpose, with its greatest value appearing in
environments where developers actively contribute to documentation.

### Automation Opportunities
Also documentation as code enables valuable automation, particularly for autogenerated documentation that lives alongside the code. (e.g.incorrect autogenerated documentation actually
can catch software errors before release). Also keeping documentation in the same repository as code significantly simplifies testing purposes and automation.


**Please also check:**

[Slide used in the video](https://docs.google.com/presentation/d/1R6N8rZbV1cfWAcyGep-p8pyylFAwWjfIYsONILRNm8M/edit?slide=id.p#slide=id.p)



