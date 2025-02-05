# Parts lifecycle

Parts, alongside [plugins](/t/snapcraft-plugins/4284), are a key component in any [Snapcraft](/t/snapcraft-overview/8940) project.

See [Adding parts](/t/adding-parts/11473) for a general overview of what parts are and how to use them, [Scriptlets](https://forum.snapcraft.io/t/scriptlets/4892) for details on how they can be scripted outside of _snapcraft.yaml_, and <!--TO DO: Path to be added when the page is created--> [Parts lifecycle]() reference for the summarised information of this page.

When creating more advanced snaps or troubleshooting build issues, a more thorough understanding of these steps and the components surrounding them may be needed.

- [Lifecycle](#heading--lifecycle)
- [Commands](#heading--commands)
- [Directories](#heading--directories)
- [Overriding a step](#heading--overriding)
- [Processing order and dependencies](#heading--processing-order)

---
<h3 id='heading--lifecycle'>Lifecycle<sup><a href=#heading--lifecycle>⚓</a></sup></h3>

A part goes through the following five steps:

1. **Pull**: downloads or otherwise retrieves the components needed to build the part. 
    * You can use the [`source-*`](/t/snapcraft-parts-metadata/8336#heading--source) keywords of a part to specify which components to retrieve. For instance, if `source` points to a Git repository, the pull step will clone that repository.
1. **Build**: constructs the part from the previously pulled components. 
    * The [`plugin`](/t/snapcraft-plugins/4284) of a part specifies how it is constructed. The [`meson`](/t/the-meson-plugin/8623) plugin, for example, executes `meson` and `ninja` to compile source code. 
    * Each part is built in a separate directory, but it can use the contents of the staging area if it specifies a dependency on other parts using the `after` keyword. See [Processing order and dependencies](#heading--processing-order) for more information.
    * To rename or move files, you can use the [`organize`](/t/snapcraft-yaml-schema/4276#p-21225-organize-79) keyword.
1. **Stage**: copies the built components into the staging area. 
    * This is the first time all the different parts that make up the snap are actually placed in the same directory. If multiple parts provide the same file with differing contents, you will get a conflict. You can avoid these conflicts by using the [`stage`](/t/snapcraft-parts-metadata/8336#heading--stage) keyword to enable or block files coming from the part. You can also use this keyword to filter out files that are not required in the snap itself, for example build files specific to a single part.
1. **Prime**: copies the staged components into the priming area. 
    * This is very similar to the stage step, but files go into the priming area instead of the staging area. The `prime` step exists because the staging area might still contain files that are required for the build but not for the snap. For example, if you have a part that downloads and installs a compiler, then you stage this part so other parts can use the compiler during building. You can then use the `prime` filter keyword to make sure that it doesn't get copied to the priming area, so it's not taking up space in the snap. 
    * To apply specific permissions to files, you can use the `permissions` keyword.
    * Some extra checks are also run during this step to ensure that all dependencies are satisfied for a proper run time. If confinement was set to `classic`, then files will be scanned and, if needed, patched to work with this confinement mode.
1. **Pack**: takes the entire contents of the `prime` directory and packs it into [a snap](/t/the-snap-format/698).

<h3 id='#heading--commands'>Commands<sup><a href=##heading--commands>⚓</a></sup></h3>

Each of these lifecycle steps can be run from the command line, and the command can be part specific or be applied to all parts in a project.

1. `snapcraft pull [<part-name>]`
1. `snapcraft build [<part-name>]`
1. `snapcraft stage [<part-name>]`
1. `snapcraft prime [<part-name>]`
1. `snapcraft pack` or `snapcraft`

Note that each command also executes the previous lifecycle steps, so `snapcraft` executes all the lifecycle steps chained together.

To access the part environment at any stage, add the `--shell` argument. For example, `snapcraft prime --shell` will run up to the *prime* step and open a shell. See [Iterating over a build](/t/iterating-over-a-build/12143) for more details.

<h3 id='heading--directories'>Directories<sup><a href=#heading--directories>⚓</a></sup></h3>

When running through its lifecycle steps, a part will use different working directories. The directories' names closely follow the steps' names.

| Environment variable | Directory | Purpose |
|--|--|--|
| `CRAFT_PART_SRC` | **`parts/<part-name>/src`** | the location of the source after the *pull* step |
| `CRAFT_PART_BUILD` | **`parts/<part-name>/build`** | the working directory during the *build* step |
| `CRAFT_PART_INSTALL`| **`parts/<part-name>/install`** | contains the results of the *build* step and the stage packages. It is also the directory where the `organize` event renames the built files |
| `CRAFT_STAGE` | **`stage`** | shared by all parts, this directory contains the contents of each part's `CRAFT_PART_INSTALL` after the *stage* step. It can contain development libraries, headers, and other components (e.g. pkgconfig files) that need to be accessible from other parts |
| `CRAFT_PRIME` | **`prime`** | shared by all parts, this directory holds the final components after the *prime* step |
| `CRAFT_PROJECT_DIR` | path to the current project's subtree in the filesystem. | path to the resulting snap after the *pack* step |

Please note that directories are specified by their environment variables:
- **Snapcraft 7+**: environment variables start with `CRAFT_`
- **Snapcraft 6, and earlier**: variables start with `SNAPCRAFT_` but are otherwise identical

See [Parts environment variables](/t/parts-environment-variables/12271) for more details.

<!--
| Step | Explanation | Source directory | Result directory |
|--|--|--|--|
| **pull** | downloads and retrieves the sources | *as specified by [`source`](https://forum.snapcraft.io/t/snapcraft-parts-metadata/8336#heading--source) key* | CRAFT_PART_**SRC** |
| **build** <br> *organise*  | builds the part <br> renames built files | CRAFT_PART_**BUILD** <br> CRAFT_PART_**INSTALL** | CRAFT_PART_**INSTALL** <br> CRAFT_PART_**INSTALL** |
| **stage** | copies built files to shared stage directory | CRAFT_PART_**INSTALL** | CRAFT_**STAGE** |
| **prime** | copies staged files to shared prime directory | CRAFT_PART_**INSTALL*** | SNAPCRAFT_**PRIME** |
| **snap** | packs contents of prime directory into a snap | CRAFT_**PRIME** | CRAFT_PROJECT_DIR |
-->

<h3 id='heading--overriding'>Overriding a step<sup><a href=#heading--overriding>⚓</a></sup></h3>

Each plugin defines the default actions that happen during a step. This behavior can be changed in two ways:
- By using `override-<step-name>` in `snapcraft.yaml`. See [Overriding steps](/t/scriptlets/4892) for more details.
- By using a local plugin.  This can inherit the parent plugin or scaffolding from the original. See [Local plugins](/t/writing-local-plugins/5125) for more details. Please note that using a local plugin has been deprecated in `base: core20` and fully removed in `base: core22`.

See [Parts environment variables](/t/parts-environment-variables/12271) for a list of part-specific environment variables that can be accessed to help build a part.

<h3 id='heading--processing-order'>Processing order and dependencies<sup><a href=#heading--processing-order>⚓</a></sup></h3>

By default, `snapcraft` runs the same lifecycle step of all parts before moving to the next step. However, you can change this behavior using the `after` keyword in the definition of a part in `snapcraft.yaml`. This creates a dependency chain from one part to another.

The ordering rules are as follows:
- Parts are ordered alphabetically by name.
- When a part requires another part (using the ``after`` keyword), the required part will go through the build and stage steps before the initial part can run its build step. 

Note that each lifecycle step depends on the completion of the previous step for that part, so to reach a desired step, all prior steps need to have successfully run. Renaming or adding parts and modifying or removing ``after`` keywords can change that order.

<h4>Example 1 - Parts run alphabetically</h4>

```yaml
 parts:
    A:
      plugin: go
    C:
      plugin: go
    B:
      plugin: go
```

```text
Pulling A
Pulling B
Pulling C
Building A
Building B
Building C
Staging A
Staging B
Staging C
...
```

<h4>Example 2 - A part has a dependency</h4>

```yaml
 parts:
    A:
      plugin: go
      after:
        - C 
    C:
      plugin: go
    B:
      plugin: go
```

```text
Pulling C
Pulling A
Pulling B
Building C
Skipping pull for C (already ran)
Skipping build for C (already ran)
Staging C (required to build 'A')
Building A
Building B
Skipping stage for C (already ran)
Staging A
Staging B
...
```

In the above example, the part named `A` is built after the part named `C` has been successfully built _and_ staged.
