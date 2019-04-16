# 💻 OpenST Developer Guidelines

## General Project Setup

* GitHub pull requests must require at least one review to allow merging.
* GitHub pull requests may only be merged using "merge commits" (no squash or rebase).
* All projects should have continuous integration setup using [TravisCI].
  * [TravisCI] must run unit tests and integration tests for every pull request.
  * [TravisCI] should run all applicable linters for every pull request.
  * GitHub pull requests must require that [TravisCI] passes to allow merging.
* All projects should have a `.gitignore` file that includes at least all build artifacts and auto-generated code (like `./build` or some generated content of `./lib`) and all dependencies (like `./node_modules`).
* Documentation should be in markdown format.
* Sign all your git commits and tags with your private key (and add the public key to your GitHub account).

Branches:

* `master` is the latest stable release.
* `develop` is the main branch and always reflects the latest development.
* `release-x.y` branches follow their respective release.

Every project must have the following files:

* `README.md`
  * For a layout of the readme see this [readme example].
  * Content should include (in this order):
    * Title followed by relevant badges using shields.io.
    * Introductory text.
    * *Instructions* section with help on installation and usage.
    * *Related Work* section.
    * *Contributing* section with.
      * help on setup and testing.
      * a link to this guide.
      * links to [discourse] and [gitter].
* `LICENSE`
* `CHANGELOG.md`
  * For the layout of the changelog see this [changelog example].
  * Notable changes should link to their pull request(s) using absolute links.

Ports:

If your project runs processes that require ports on the host, for example ganache, the following ports should be used.
We define ranges here so that you can select one or more ports inside the range *that should not conflict with the ports of the other repositories.*

* Ganache: `50000 - 50999`
* Integration test containers: `51000 - 51999`
* Other processes: `52000 - 52999`

This should avoid port conflicts as much as possible.
It should furthermore rule out the case where, for example, tests of a repository pass or fail because another, differently configured ganache from another repository is still running.

## Releases

Version identifiers follow [Semantic Versioning 2] without a leading `v`.
A valid version identifier would be for example `0.10.3` or `0.5.1-alpha.3`.

Every **MINOR** version must have its dedicated git branch.
For example, versions `0.10.x` would go into the release branch `release-0.10` whereas versions `1.2.x` would go into release branch `release-1.2`.
Release branches must start with `release-` followed by **MAJOR** and **MINOR** version identifier separated by a period.

Every release that shall be published must be tagged in git (on GitHub).
Pre-releases (e.g. `-alpha.3`) must only have an annotated tag.
Releases (e.g. `0.10.0`) must have an accompanying GitHub release with release notes.

Before merging the latest changes from `develop` into a release branch:

* Every release and pre-release must make sure that the `CHANGELOG` reflects all relevant changes and is up-to-date.
* Every release and pre-release must make sure that the version listed in `package.json` is correct.

## Solidity

* Follow our [Solidity style guide].
* Any project with Solidity code should have [ethlint] (formerly solium) installed.
  * Use this configuration: [soliumrc].
  * Make sure all changes pass the linter before opening a pull request.

### Directory Structure

* All contract source code should be below the directory `./contracts`.
* All unit test and related code should be below the directory `./test`.
* All integration test and related code should be below the directory `./test_integration`.
* All build artifacts of a library project should be in the directory `./lib`.
* All documentation should be below the directory `./docs`.

## JavaScript

* Follow the [Airbnb style guide].
* Any project with JavaScript code should have [eslint] installed.
  * Use this configuration: [eslintrc].
  * Make sure all changes pass the linter before opening a pull request.

### Directory Structure

* All source code should be below the directory `./src`.
* All unit test and related code should be below the directory `./test`.
* All integration test and related code should be below the directory `./test_integration`.
* All build artifacts of a library project should be in the directory `./lib`.
* All documentation should be below the directory `./docs`.

### Logging 

* [Winston](https://github.com/winstonjs/winston) should be used as logging library.
* By default logging to console should be enabled .
* It should be possible to switch from console to file logging by updating an environment variable. 
```bash
export ENABLE_PROJECT_NAME_LOGGER=true

```
* Logger should support below log levels:
    * error
    * warn
    * info
    * verbose
    * debug
    * silly

* Default log level should be `info`.  
* It should be possible to change log level by updating an environment variable.   
```bash
export ENABLE_PROJECT_NAME_LOG_LEVEL=error

```
* Default log location should be `${PWD}/log`.
* It should be able to change log location by updating an environment variable.  
  ```
  export PROJECT_NAME_LOGGER_PATH=/path
  ```
## `package.json`

This section applies to Solidity and JavaScript projects alike.

* `package.json` should include all required build steps in `scripts.prepack`.
* Provide at least the following `package.json` scripts:
  * `test`
  * `test:integration`
  * `build`
  * `prepack`
    * Should run all steps required before packing (publishing) the project (probably at least `npm run build`).
    * When you use a packaging tool like [webpack], make sure that `npm ci` is part of the `prepack` script.
  * `lint`
* Define `files` to minimize the size of the published package.
* For library projects, `main` should point to `./lib/index.js`.

## Rust

* Format your code using `rustfmt`.

[airbnb style guide]: https://github.com/airbnb/javascript
[changelog example]: https://github.com/OpenSTFoundation/mosaic-contracts/blob/develop/CHANGELOG.md
[discourse]: https://discuss.openst.org/
[eslint]: https://eslint.org/
[eslintrc]: ./.eslintrc.json
[ethlint]: https://github.com/duaraghav8/Ethlint
[gitter]: https://gitter.im/OpenSTFoundation/SimpleToken
[readme example]: https://github.com/OpenSTFoundation/mosaic.js/blob/develop/README.MD
[semantic versioning 2]: https://semver.org/
[solidity style guide]: ./SOLIDITY_STYLE_GUIDE.md
[soliumrc]: ./.soliumrc.json
[travisci]: https://travis-ci.org/
[webpack]: https://webpack.js.org/

## Release Process

* First version to be released is alpha version, followed by beta version.
*  Naming convention followed for alpha and beta version is `x.y.z-alpha.alphaversion` and `x.y.z-beta.betaversion`. The value of alpha version/beta version should be incremental starting from 1.
Example :
``` 
    Consider the version to be released is 0.9.0. If we want to release alpha version for it, then the name of the version should be 0.9.0-alpha.1 and if we want to release beta version then the name for it should be 0.9.0-beta.1 .
```
* Naming convention for hotfix branch should be hotfix-x.y.z where x.y represents the release version number where the bug is found and z represents incremental patch version for the release starting from 1.

1. For each version to be released(except hotfix release), below steps should be followed :
    * Create branch release-x.y, where x and y represents the version numbers, from develop.
    * Create a tag from release-x.y. If alpha/beta version is to be released then the tag name should include alpha/beta as per the naming convention mentioned.
    * Integrate and test the alpha/beta version in JLP.
    * Organise external security, once everything in integration works fine.
    * Release which have been reviewed by external auditor, a rcversion for it should be released. Naming convention for rcversion is `x.y.z-rc.rcversion`. The value of `rcversion` is incremental starting from 1.
    * We should then merge the branch release-x.y to develop and master branches.
    * To release-x.y, create a tag named as x.y.0 .

2.  For each hotfix version to be released, below steps should be followed :
    * Create a branch from master if the bug is in the current released version.Otherwise create a branch from the tag where the bug is found. Name of the branch should be as mentioned.
    * Commit the fix to the hotfix branch.
    * After committing, test the hotfix in JLP.
    * Merge the hotfix branch to master and develop. If the release branch currently exists, then the hotfix branch should be merged into that release branch instead of develop.
    * Create a tag from master. Tag naming conversion should be x.y.z where x.y represents the release version where the bug was found and z’s value should be incremental.

