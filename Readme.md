
# No longer maintained
 This is restored from a local copy of what used to be https://github.com/apex/apex which seems to have been deleted as of September 2 2020.

This software is no longer being maintainted and should not be chosen for new projects, unless your company is interested in sponsoring the project it's unfortunately unsustainable. See this [issue](https://github.com/apex/apex/issues/932) for more information

![Apex Serverless Architecture](assets/title.png)

Apex lets you build, deploy, and manage [AWS Lambda](https://aws.amazon.com/lambda/) functions with ease. With Apex you can use languages that are not natively supported by AWS Lambda through the use of a Node.js shim injected into the build. A variety of workflow related tooling is provided for testing functions, rolling back deploys, viewing metrics, tailing logs, hooking into the build system and more.

This project is designed for event-driven pipelines as it does not abstract away FaaS (functions as a service). If you are building web applications, APIs, or sites, consider using [Apex Up](https://github.com/apex/up), which provides a more out-of-the-box experience for these use-cases.

# Release new version:

This uses goreleaser to release new versions, follow instructions here https://goreleaser.com/quick-start/

## Installation

On macOS, Linux, or OpenBSD run the following:

```
curl https://raw.githubusercontent.com/outline-insurance/apex/master/install.sh | sh
```

Note that you may need to run the `sudo` version below, or alternatively chown `/usr/local`:
```
curl https://raw.githubusercontent.com/outline-insurance/apex/master/install.sh | sudo sh
```

On Windows download [binary](https://github.com/outline-insurance/apex/releases).

After downloading, rename binary file 'apex.exe', then add to PATH.

If already installed, upgrade with:

```
apex upgrade
```

## Runtimes

Currently supports:

- Node.js
- Golang
- Python
- Ruby
- Java
- Rust
- Clojure

Example projects for all supported runtimes can be found in [_examples](_examples) directory.

## Features

- Supports languages Lambda does not natively support via shim
- Binary install (install apex quickly for continuous deployment in CI etc)
- Hook support for running commands (transpile code, lint, dependency management, etc)
- Batteries included but optional (opt-in to higher level abstractions)
- Environment variable population via command-line, file, or inline config
- Idempotent deployments (checksums skip already-deployed code)
- Multiple environments via `project.ENV.json` and `function.ENV.json` files
- Configuration inheritance and overrides
- Command-line function invocation with JSON streams
- Command & function name autocompletion
- Function name globbing (ex: `apex deploy api_*`)
- Transparently generates a zip for your deploy
- Project bootstrapping with optional Terraform support
- Function metrics and cost analysis
- Ignore deploying files with .apexignore
- Function rollback support
- Tail function logs
- Concurrency for quick deploys
- Dry-run to preview changes
- VPC support
- Multiple region support
- Lambda@Edge support

## Example

Apex projects are made up of a `project.json` configuration file, and zero or more Lambda functions defined in the "functions" directory. Here's an example file structure:

```
project.json
functions
├── bar
│   ├── function.json
│   └── index.js
└── foo
    ├── function.json
    └── index.js
```

The `project.json` file defines project level configuration that applies to all functions, and defines dependencies. For this simple example the following will do:

```json
{
  "name": "example",
  "description": "Example project"
}
```

Each function uses a `function.json` configuration file to define function-specific properties such as the runtime, amount of memory allocated, and timeout. This file is completely optional, as you can specify defaults in your `project.json` file. For example:

```json
{
  "name": "bar",
  "description": "Node.js example function",
  "runtime": "nodejs4.3",
  "memory": 128,
  "timeout": 5,
  "role": "arn:aws:iam::293503197324:role/lambda"
}
```

Now the directory structure for your project would be:

```
project.json
functions
├── bar
│   └── index.js
└── foo
    └── index.js
```

Finally the source for the functions themselves look like this in Node.js:

```js
console.log('start bar')
exports.handle = function(e, ctx) {
  ctx.succeed({ hello: e.name })
}
```

Apex operates at the project level, but many commands allow you to specify specific functions. For example you may deploy the entire project with a single command:

```
$ apex deploy
```

Or whitelist functions to deploy:

```
$ apex deploy foo bar
```

Invoke it!

```
$ echo '{ "name": "Tobi" }' | apex invoke bar
{ "hello": "Tobi" }
```

See the [Documentation](http://apex.run) for more information.

## Links

- [Website](http://apex.run)
- [Medium](https://medium.com/@tjholowaychuk)
- [Slack](https://chat.apex.sh/)

---

[![Build Status](https://semaphoreci.com/api/v1/projects/d27ff350-b9c5-4d99-96e5-64b1afb441c5/649392/badge.svg)](https://semaphoreci.com/tj/apex)
[![Slack Status](https://apex-slackin.herokuapp.com/badge.svg)](https://apex-slackin.herokuapp.com/)
[![GoDoc](https://godoc.org/github.com/outline-insurance/apex?status.svg)](https://godoc.org/github.com/outline-insurance/apex)
![](https://img.shields.io/badge/license-MIT-blue.svg)
![](https://img.shields.io/badge/status-stable-green.svg) [![OpenCollective](https://opencollective.com/apex/backers/badge.svg)](#backers) [![OpenCollective](https://opencollective.com/apex/sponsors/badge.svg)](#sponsors)

<a href="https://apex.sh"><img src="http://tjholowaychuk.com:6000/svg/sponsor"></a>
