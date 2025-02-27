# Tamarin

[![CircleCI](https://dl.circleci.com/status-badge/img/gh/cloudcmds/tamarin/tree/main.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/cloudcmds/tamarin/tree/main)
[![MIT license](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Go.Dev reference](https://img.shields.io/badge/go.dev-reference-blue?logo=go&logoColor=white)](https://pkg.go.dev/github.com/cloudcmds/tamarin)
[![Go Report Card](https://goreportcard.com/badge/github.com/cloudcmds/tamarin?style=flat-square)](https://goreportcard.com/report/github.com/cloudcmds/tamarin)
[![Releases](https://img.shields.io/github/release/cloudcmds/tamarin/all.svg?style=flat-square)](https://github.com/cloudcmds/tamarin/releases)

A fun and pragmatic embedded scripting language written for Go projects.
Import it as a library or try out the Tamarin REPL.

## Documentation

Documentation is available at [cloudcmds.github.io/tamarin](https://cloudcmds.github.io/tamarin/).

## Getting Started

The [Quick Start](https://cloudcmds.github.io/tamarin/quick-start/) in the documentation
is where you should head to get started.

If you use Homebrew, you can install the Tamarin CLI as follows:

```
brew tap cloudcmds/tamarin
brew install tamarin
```

Having done that, just run `tamarin` to start the CLI or `tamarin -h` to see
usage information.

## Discuss the Project

Please visit the [Github discussions](https://github.com/cloudcmds/tamarin/discussions)
page to share thoughts and questions.

## Feature Overview

- Familiar syntax inspired by Go, Typescript, and Python
- Growing standard library which generally wraps the Go stdlib
- Includes higher level libraries that are beyond the Go stdlib
- Currently libraries include: `json`, `math`, `rand`, `strings`, `time`, `uuid`, `strconv`, `pgx`
- Built-in types include: `set`, `map`, `list`, `result`, and more
- Functions are values; closures are supported
- Cancel evaluation using Go contexts
- Library may be imported using the `import` keyword
- Easy HTTP requests via the `fetch` built-in function
- Pipeline expressions to create processing chains
- Error handling inspired by Rust, using a Result type
- String templates similar to Python's f-strings

## Language Features and Syntax

See [docs/Features.md](./docs/Features.md).

## Syntax Highlighting

A [Tamarin VSCode extension](https://marketplace.visualstudio.com/items?itemName=CurtisMyzie.tamarin-language)
is already available which currently only offers syntax highlighting.

You can also make use of the [Tamarin TextMate grammar](./vscode/syntaxes/tamarin.grammar.json).

![](docs/assets/syntax-highlighting.png?raw=true)

## Further Documentation

Work in progress. See [assorted.tm](./examples/assorted.tm).

## Contributing

🎉 This project is just getting started. Tamarin is intended to be a community
project and you can lend a hand in many different ways.

- Please ask questions and share ideas in [Github discussions](https://github.com/cloudcmds/tamarin/discussions)
- Share Tamarin on any social channels that may appreciate it
- Let us know about any usage of Tamarin that we can celebrate
- Leave us a star us on Github

## Known Issues & Limitations

- File I/O was intentionally omitted for now
- No concurrency support yet
- No user-defined types yet

## Pipe Expressions

A single value may be passed between successive calls using pipe expressions.

```
array := ["gophers", "are", "burrowing", "rodents"]

sentence := array | strings.join(" ") | strings.to_upper

print(sentence)
```

Output:

```
GOPHERS ARE BURROWING RODENTS
```

The intent is that if any call in the pipeline returns a `Result` object, that
object is unwrapped before providing it to the next call (or the pipeline aborts
if the Result is an error). This is not yet implemented.

## Error Handling in Tamarin

There are two categories of errors in Tamarin. Severe errors which indicate a programming mistake
create `*object.Error` errors that abort evaluation immediately. These are similar to Go panics.
Operations that may fail return an `*object.Result` object that contains either an `Ok` value or
an `Err` value. This is inspired by [Rust's Result type](https://doc.rust-lang.org/std/result/).
`Result` objects proxy to the `Ok` value in certain situations, to keep adhoc scripting concise.

### Result Methods

- `is_err`: returns `true` if the Result contains an error
- `is_ok`: returns `true` if the Result contains the successful operation value
- `unwrap`: returns the ok value if present and errors otherwise
- `unwrap_or`: returns the ok value if present and the default value otherwise
- `expect`: returns the ok value if present or raises an error with the given message

Other method calls on the `Result` object proxy to calls on the ok value (or
raise an error).

Altogether this means programmers have reliable tools for handling errors as
values without requiring `try: catch` style exceptions. In quick scripts, you
can rely on `Result` method proxying if you want, while in situations requiring
robustness you can use the `Result` methods to check for and unwrap errors.

## Credits

- [Thorsten Ball](https://github.com/mrnugget) and his book [Writing an Interpreter in Go](https://interpreterbook.com/).
- [Steve Kemp](https://github.com/skx) and the work in [github.com/skx/monkey](https://github.com/skx/monkey).

See more information in [CREDITS](./CREDITS).

## License

Released under the MIT License. Copyright Curtis Myzie / [github.com/myzie](https://github.com/myzie).
