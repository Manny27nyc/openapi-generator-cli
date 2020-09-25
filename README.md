# @openapitools/openapi-generator-cli

[![Join the Slack chat room](https://img.shields.io/badge/Slack-Join%20the%20chat%20room-orange)](https://join.slack.com/t/openapi-generator/shared_invite/enQtNzAyNDMyOTU0OTE1LTY5ZDBiNDI5NzI5ZjQ1Y2E5OWVjMjZkYzY1ZGM2MWQ4YWFjMzcyNDY5MGI4NjQxNDBiMTlmZTc5NjY2ZTQ5MGM)

[![Renovate enabled](https://img.shields.io/badge/renovate-enabled-brightgreen.svg)](https://renovateapp.com/)
[![Build Status](https://travis-ci.org/openapitools/openapi-generator-cli.svg?branch=master)](https://travis-ci.org/OpenAPITools/openapi-generator-cli)
[![HitCount](http://hits.dwyl.io/openapitools/openapi-generator-cli.svg)](http://hits.dwyl.com/openapitools/openapi-generator-cli)

OpenAPI Generator allows generation of API client libraries (SDK generation), server stubs, documentation and 
configuration automatically given an OpenAPI Spec (both 2.0 and 3.0 are supported). Please see
[OpenAPITools/openapi-generator](https://github.com/OpenAPITools/openapi-generator)

---

## Version 2.x.x

#### [update] The command has been renamed

You need to execute `openapi-generator-cli` instead of `openapi-generator` from now on.

#### [added] [semver](https://semver.org/) is support! 🎉

<p align="center"><img src="/img/vm.gif?raw=true"/></p>

To make that happen, a version management was added to the package.
The first time you run the command `openapi-generator-cli` the last stable version 
of [OpenAPITools/openapi-generator](https://github.com/OpenAPITools/openapi-generator) is downloaded by default. 

That version is saved in the file *openapitools.json*. Therefore you should include this file in your version control, 
to ensure that the correct version is being used next time you call the command.

If you would like to use a different version of the [OpenAPITools/openapi-generator](https://github.com/OpenAPITools/openapi-generator), 
you could change it by using one of the following commands:
 
- `openapi-generator-cli version-manager list` 
- `openapi-generator-cli version-manager set  <versionTags...>`

#### [added] generator config

You will now be able to configure the code generation in *openapitools.json*. 
This makes it more convenient to generate code for every file that matches the given glob expression.
For more information, [please check out the configuration documentation blow](#configuration).

## Installation

### Locally (recommended)

```sh
npm install @openapitools/openapi-generator-cli
```

or using yarn

```sh
yarn add @openapitools/openapi-generator-cli
```

After the installation has finished you can run `npx openapi-generator-cli` or add a script like this:

```json
{
  "name": "my-cool-package",
  "version": "0.0.0",
  "scripts": {
    "my-awesome-script-name": "openapi-generator-cli generate -i docs/openapi.yaml -g typescript-angular -o generated-sources/openapi --additional-properties=ngVersion=6.1.7,npmName=restClient,supportsES6=true,npmVersion=6.9.0,withInterfaces=true",
  }
}
```

Note the whitespace sensitivity when using multiple additional-properties:

```text
--additional-properties=ngVersion=6.1.7,npmName=restClient,supportsES6=true,npmVersion=6.9.0,withInterfaces=true
```

### Globally

```sh
npm install -g @openapitools/openapi-generator-cli
```

or using yarn

```sh
yarn global add @openapitools/openapi-generator-cli
```

After the installation has finished you can run `openapi-generator-cli`

## Usage

Mac/Linux:
```
openapi-generator-cli generate -g ruby -i https://raw.githubusercontent.com/OpenAPITools/openapi-generator/master/modules/openapi-generator/src/test/resources/3_0/petstore.yaml -o /var/tmp/ruby-client
```

Windows:
```
openapi-generator-cli generate -g ruby -i https://raw.githubusercontent.com/OpenAPITools/openapi-generator/master/modules/openapi-generator/src/test/resources/3_0/petstore.yaml -o C:\temp\ruby-client
```

## Configuration

If you have installed the package locally and executed the command `openapi-generator-cli` at least once, 
you will find a new file called *openapitools.json* along with the *package.json*. **Please add this file to your VCS.** 

Initially the file has the following content:

```json5
{
  "$schema": "node_modules/@openapitools/openapi-generator-cli/config.schema.json",
  "spaces": 2,
  "generator-cli": {
    "version": "4.3.1" // or the current latest version ;)
  }
}
```

This configuration indicates the following:

- the json file shall be formatted using **2 spaces**
- the generator-cli version 4.3.1 is used

Further it is also possible to configure generators, for example:

```json5
{
  "$schema": "node_modules/@openapitools/openapi-generator-cli/config.schema.json",
  "spaces": 2,
  "generator-cli": {
    "version": "4.3.1",
    "generators": {
      "v2.0": { // any name you like (just printed to the console log) 
        "generatorName": "typescript-angular",
        "output": "#{cwd}/output/v2.0/#{ext}/#{name}",
        "glob": "examples/v2.0/{json,yaml}/*.{json,yaml}",
        "additionalProperties": {
          "ngVersion": "6.1.7",
          "npmName": "restClient",
          "supportsES6": "true",
          "npmVersion": "6.9.0",
          "withInterfaces": true
        }
      },
      "v3.0": { // any name you like (just printed to the console log) 
        "generatorName": "typescript-fetch",
        "output": "#{cwd}/output/v3.0/#{ext}/#{name}",
        "glob": "examples/v3.0/petstore.{json,yaml}"
      }
    }
  }
}
```

If `openapi-generator-cli generate` is called without further arguments, then the configuration 
is automatically used to generate your code. 🎉


##### Available placeholders
     
| placeholder  | description                                                   | example                                               |
|--------------|---------------------------------------------------------------|-------------------------------------------------------|
| name         | just file name                                                | auth                                                  |
| Name         | just file name, but starting with a capital letter            | Auth                                                  |
| cwd          | the current cwd                                               | /Users/some-user/projects/some-project                |
| base         | file name and extension                                       | auth.yaml                                             |
| path         | full path and filename                                        | /Users/some-user/projects/some-project/docs/auth.yaml |
| dir          | path without the filename                                     | /Users/some-user/projects/some-project/docs           |
| relDir       | directory name of file relative to the glob provided          | docs                                                  |
| relPath      | file name and extension of file relative to the glob provided | docs/auth.yaml                                        |
| ext          | just file extension                                           | yaml                                                  |

## Further Documentation

Please refer to the [official openapi-generator docs](https://github.com/OpenAPITools/openapi-generator#3---usage) for
more information about the possible arguments and a detailed usage manual of the command line interface.

## You like the package?

Please leave a star.
