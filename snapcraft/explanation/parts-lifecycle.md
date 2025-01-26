# Parts lifecycle
Parts, alongside [plugins](/t/snapcraft-plugins/4284), are a key component in any [Snapcraft](/t/snapcraft-overview/8940) project.

See [Adding parts](/t/adding-parts/11473) for a general overview of what parts are and how to use them and [Scriptlets](https://forum.snapcraft.io/t/scriptlets/4892) for details on how they can be scripted outside of _snapcraft.yaml_.

All parts within a project, by means of the logic encoded the plugins they're using, all go through the same series of steps. Knowing these steps, and which directories are used for each step, can help when creating more advanced snaps, and when troubleshooting build issues.

- [Lifecycle steps](#heading--steps)
- [Step dependencies](#heading--step-dependencies)
- [Parts directories](#heading--parts-directories)

---

<h3 id='heading--steps'>Lifecycle steps<sup><a href=#heading--steps>⚓</a></sup></h3>

The steps a part goes through are as follows:

1. **pull**: downloads or otherwise retrieves the components needed to build the part. You can use the [`source-*` keywords](/t/snapcraft-parts-metadata/8336#heading--source) of a part to specify which components to retrieve. If `source` points to a git repository, for example, the pull step will clone that repository.
1. **build**: constructs the part from the previously pulled components. The [`plugin`](/t/snapcraft-plugins/4284) of a part specifies how it is constructed. The [`meson` plugin](/t/the-meson-plugin/8623), for example, executes `meson` and `ninja` to compile source code. Each part is built in a separate directory, but it can use the contents of the staging area if it specifies a dependency on other parts using the `after` keyword. See [Step dependencies](#heading--step-dependencies) for more information.
1. **stage**: copies the built components into the staging area. This is the first time all the different parts that make up the snap are actually placed in the same directory. If multiple parts provide the same file with differing contents, you will get a conflict. You can avoid these conflicts by using the [`stage` keyword](/t/snapcraft-parts-metadata/8336#heading--stage) to enable or block files coming from the part. You can also use this keyword to filter out files that are not required in the snap itself, for example build files specific to a single part.
1. **prime**: copies the staged components into the priming area, to their final locations for the resulting snap. This is very similar to the stage step, but files go into the priming area instead of the staging area. The `prime` step exists because the staging area might still contain files that are required for the build but not for the snap. For example, if you have a part that downloads and installs a compiler, then you stage this part so other parts can use the compiler during building. You can then use the `prime` filter keyword to make sure that it doesn't get copied to the priming area, so it's not taking up space in the snap. Some extra checks are also run during this step to ensure that all dependencies are satisfied for a proper run time. If confinement was set to `classic`, then files will be scanned and, if needed, patched to work with this confinement mode.

Finally, **pack** takes the entire contents of the `prime` directory and packs it into [a snap](/t/the-snap-format/698).

Each of these lifecycle steps can be run from the command line, and the command can be part specific or apply to all parts in a project.

1. `snapcraft pull [<part-name>]`
1. `snapcraft build [<part-name>]`
1. `snapcraft stage [<part-name>]`
1. `snapcraft prime [<part-name>]`
1. `snapcraft pack` or `snapcraft`

Note that each command also executes the previous lifecycle steps, so `snapcraft` executes all the lifecycle steps chained together.

To access the part environment at any stage, add the `--shell` argument. For example, `snapcraft prime --shell` will run up to the *prime* step and open a shell. See [Iterating over a build](/t/iterating-over-a-build/12143) for more details.

<h3 id='heading--step-dependencies'>Step dependencies<sup><a href=#heading--step-dependencies>⚓</a></sup></h3>

Each lifecycle step depends on the completion of the previous step for that part, so to reach a desired step, all prior steps need to have successfully run. By default, `snapcraft` runs the same lifecycle step of all parts before moving to the next step. However, you can change this behavior using the `after` keyword in the definition of a part in `snapcraft.yaml`. This creates a dependency chain from one part to another.

```yaml
 grv:
    plugin: go
    go-channel: 1.11/stable
    after:
      - libgit2
```

In the above example, the part named `grv` will be built after the part named `libgit2` has been successfully built _and_ staged.

<h3 id='heading--overriding-steps'>Overriding a step<sup><a href=#heading--overriding-steps>⚓</a></sup></h3>

Each plugin defines the default actions that happen during a step. This behavior can be changed in two ways.

- By using `override-<step-name>` in `snapcraft.yaml`. See [Overriding steps](/t/scriptlets/4892) for more details.
- By using a local plugin.  This can inherit the parent plugin or scaffolding from the original. See [Local plugins](/t/writing-local-plugins/5125) for more details.

See [Parts environment variables](/t/parts-environment-variables/12271) for a list of part-specific environment variables that can be accessed to help build a part.

<h3 id='heading--parts-directories'>Parts directories<sup><a href=#heading--parts-directories>⚓</a></sup></h3>

When running through its build steps, a part will use different working directories. These closely follow the step names for the lifecycle.

The directories are specified by their environment variables:
- **Snapcraft 7+**: environment variables start with `CRAFT_`
- **Snapcraft 6, and earlier**: variables start with `SNAPCRAFT_` but are otherwise identical

See [Parts environment variables](/t/parts-environment-variables/12271) for more details.

| Environment variable | Directory&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Purpose |
|--|--|--|
| `CRAFT_PART_SRC` | **`parts/<part-name>/src`** | the location of the source during the *pull* step |
| `CRAFT_PART_BUILD` | **`parts/<part-name>/build`** | the working directory during the *build* step |
| `CRAFT_PART_INSTALL`| **`parts/<part-name>/install`** | contains the results of the *build* step and the stage packages. |
| `CRAFT_STAGE` | **`stage`** | shared by all parts, this directory contains the development libraries, headers, and other components (e.g.; pkgconfig files) that need to be accessible from other parts |
| `CRAFT_PRIME` | **`prime`** | shared by all parts, this directory holds the final components for the resulting snap. |

The following table gives an overview of which directories each step uses. 

<!--
| Step | Explanation | Source&nbsp;directory&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Result directory |
|--|--|--|--|
| **pull** | downloads and retrieves the sources | *as specified by [`source`](https://forum.snapcraft.io/t/snapcraft-parts-metadata/8336#heading--source) key* | CRAFT_PART_**SRC** |
| **build** <br> *organise*  | builds the part <br> renames built files | CRAFT_PART_**BUILD** <br> CRAFT_PART_**INSTALL** | CRAFT_PART_**INSTALL** <br> CRAFT_PART_**INSTALL** |
| **stage** | copies built files to shared stage directory | CRAFT_PART_**INSTALL** | CRAFT_**STAGE** |
| **prime** | copies staged files to shared prime directory | CRAFT_PART_**INSTALL*** | SNAPCRAFT_**PRIME** |
| **snap** | packs contents of prime directory into a snap | CRAFT_**PRIME** | CRAFT_PROJECT_DIR |
-->

| Step | Explanation |
|--|--|
| **pull** | downloads and retrieves the sources specified by the [`source`](https://forum.snapcraft.io/t/snapcraft-parts-metadata/8336#heading--source) key and puts them in CRAFT_PART_**SRC** |
| **build** | builds the sources in CRAFT_PART_**BUILD** and places the result in CRAFT_PART_**INSTALL** |
| **organize** | renames built files in CRAFT_PART_**INSTALL** |
| **stage** | copies built files from CRAFT_PART_**INSTALL** to the shared CRAFT_**STAGE** |
| **prime** | copies the *staged* files from the shared CRAFT_**STAGE** to the shared CRAFT_**PRIME** |
| **pack** | packs contents of CRAFT_**PRIME** into a snap and puts the snap in CRAFT_PROJECT_DIR |
