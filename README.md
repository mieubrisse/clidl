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
- **Stub Generation:** Function stubs to build the CLI's functionality can be generated in the language of your choice using the CLI framework of your choice.
- **Versioning:** The CLI interface can be semantically versioned, with breaking changes detected automatically by the tooling.
- **Linting:** Since CLIDL is a machine-readable spec, extra toolchains can be added on top. E.g., a tool that checks for typos in the helptext of CLI commands, or a tool that verifies all links in the helptext are valid.
- **Client Libraries:** Client libraries to call the CLI can be generated in the language of your choice. For CLIs wrapped in a container, those same client libraries can be used to run the container.
- **Validation:** While application-specific validation (e.g. make sure parameter X is less than parameter Y) still needs to be in source code, 
- **Typing:** Because typing is in a single defined place rather than being buried in the program code, fancy shells like `fish` can give hints 
- **Duplicate Config Resolution:** For CLIs that are configurable both by envvars and flags, CLIDL can define which takes precedence and autogenerate the code for making it so.
- **Metadata:** Like the OpenAPI spec, CLIDL allows for attaching metadata like description, author, and license.
- **Form Generation:** With a standardized definition of the CLI's interface, 
- **Extended Shell Support:** For more shells with more advanced support like `fish`, 
