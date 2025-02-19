# Parts lifecycle

Each part is composed of five steps, known as the "lifecycle".

See [Part lifecycle](/t/parts-lifecycle/12231) and [Parts environment variables](/t/parts-environment-variables/12271) to learn more about the Parts lifecycle and its environment variables.

| Step | Command | Purpose | Directory  | Path
| ----------- | ----------- | ----------- | ----------- | ----------- |
| **Pull** | `snapcraft pull [<part-name>]` | Downloads or retrieve the components' sources and external dependencies needed to build the part. | Components' sources and external dependencies are put in CRAFT_PART_**SRC** or CRAFT_PART_SRC_**WORK** if the source subdirectory is overridden. | **`parts/<part-name>/src`** or **`parts/<part-name>/src/<subdirectory>`**  |
| **Build** | `snapcraft build [<part-name>]` | Builds the components from the previously pulled sources. | Build the sources in CRAFT_PART_**BUILD** and places the result in CRAFT_PART_**INSTALL**. | **`parts/<part-name>/build`** and **`parts/<part-name>/install`** |
| **Stage** | `snapcraft stage [<part-name>]` | Copies the built components into the staging area. | The built components are put in CRAFT_**STAGE**. | **`stage`** |
| **Prime** | `snapcraft prime [<part-name>]` | Copies the staged components into the priming area. | The staged components are put in CRAFT_**PRIME**. | **`prime`** |
| **Pack** | `snapcraft pack or snapcraft` | Takes the contents of the prime directory and packs it into a snap. | The snap is put in CRAFT_PROJECT_**DIR**. | The path to the current project’s subtree in the filesystem. |
