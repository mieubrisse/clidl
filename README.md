CLIDL
=====
Background
----------
API definition languages like OpenAPI and Protobuf have brought order to the world of server APIs. They provide a single source of truth with sensible typing, versioning, and documentation, which allows the server to tell us how to talk it. This makes interaction friendly: we can use generated stubs to build our servers, tools like Postman to talk to them manually, and machine-generated client libraries to talk to them programmatically.

Yet, CLIs - despite being an interface - are still stuck in the Stone Age:

- Building a CLI requires learning a CLI library in your language of choice, hand-crafting a bunch of code, and dealing with the peculiarities of the library (e.g. Go's Cobra not having a built-in way to document args)
- Documentation is often scattered throughout the source code or out-of-date in the README (if it exists at all)
- Calling a CLI or container from another program requires passing in a bunch of strings, leaving your code vulnerable to runtime breaks that the compiler happily approves
- Type safety is nonexistent
- It's nearly guaranteed that a server's semantic versioning refers to its API, and not to its CLI arguments (leaving no indication if the CLI arguments cause a breaking change)
- The most widespread container management tools like Compose and Helm leave it up to you to figure out how to call the container properly
- Many CLIs support configuring the CLI through args & flags, environment variables, _and_ config files, leaving it up to the author to resolve everything together

Can we do better?

CLIDL
-----
I'd like to propose the Command Line Interface Definition Language (CLIDL), a sort of OpenAPI for CLIs, to facilitate human and machine interaction with CLIs.

Something like this:

```yaml
TODO INSERT HERE
```

Benefits of using CLIDL
-----------------------

- **Documentation Generation:** Documentation for the CLI can be generated programmatically, including valid values for each argument, error codes, and the cases where error codes exist.
- **Stub Generation:** Function stubs to build the CLI's functionality can be generated in the language of your choice using the CLI framework of your choice, leaving you free to build rather than figure out the CLI framework.
- **Client Library Generation:** Client libraries to call the CLI can be generated in the language of your choice. These will also work nearly the same for CLIs wrapped in containers. These have some extra features:
    - **Structured Output:** For commands that return information in a structured way, CLIDL will automatically generate an `--output-form` flag that can be set to `pretty` or `structured`. CLIDL-generated client libraries will automatically use the `structured` version, parsing the result into a meaningful object in the library's language. Setting to `pretty` will pass the result through an optional `render` function that can be defined in the CLI.
    - **Helptext Generation:** Allowed and default values can be automatically added to the helptext, so you don't have to remember it.
- **Versioning:** The CLI interface can be semantically versioned, with breaking changes detected automatically by the tooling.
- **Linting:** Since CLIDL is a machine-readable spec, extra toolchains can be added on top. E.g.:
    - Checking for typos in the helptext of CLI commands
    - Verifying all links in the helptext are valid
    - Verifying all commands, args, and flags have helptext
    - Verifying all flags follow a consistent naming format
- **Prevalidation:** All CLIs built using CLIDL get generated with a `--check-validity` flag, which does a lightweight run simply to check the validity of arguments. This allows fancy shells like `fish` to give realtime feedback on if a command would fail due to a validation error before the user even runs the command.
- **Duplicate Config Resolution:** For CLIs that are configurable both by envvars and flags, CLIDL can define which takes precedence and autogenerate the code for making it so.
- **Form Generation:** With a standardized definition of the CLI's interface, graphical interfaces for running the CLI can now be programmatically generated
- **Typing:** Because typing is in a single defined place rather than being buried in the program code, fancy shells like `fish` can give hints.
- **Validation:** Basic forms of validation (e.g. selection from an enum list) can be encoded in CLIDL, meaning it's clear for users and you don't have to write it in code.
- **Suggestions, Not Mandates:** CLIDL can nudge you towards a standardized way of writing CLIs (e.g. `--flag` instead of `-flag`), though won't force you to do it
- **Metadata:** Like the OpenAPI spec, CLIDL allows for attaching metadata like description, author, and license.

Generation Controls
-------------------
