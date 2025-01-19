# Parts lifecycle

Each part is composed of five steps, known as the "lifecycle":

| Step | Purpose | Directory | Command |
| ----------- | ----------- | ----------- | ----------- |
| 1. **Pull** | Download or retrieve the components' sources and external dependencies needed to build the part. | Components' sources and external dependencies are put in CRAFT_PART_**SRC**. | `snapcraft pull [<part-name>]` |
| 2. **Build** | Build the components from the previously pulled sources. | Build the sources in CRAFT_PART_**BUILD** and places the result in CRAFT_PART_**INSTALL** | `snapcraft build [<part-name>]` |
| 3. **Stage** | Copy the built components into the staging area. | The built components are put in CRAFT_**STAGE**. | `snapcraft stage [<part-name>]` |
| 4. **Prime** | Copy the staged components into the priming area. | The staged components are put in CRAFT_**PRIME**. | `snapcraft prime [<part-name>]` |
| 5. **Pack** | Take the contents of the prime directory and pack it into a snap. | The snap is put in CRAFT_PROJECT_**DIR**. | `snapcraft pack or snapcraft` |
