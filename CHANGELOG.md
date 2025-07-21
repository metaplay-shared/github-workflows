# Changelog

## [0.1.2] - 2025-07-22

* setup-cli: Use the unthrottled download URL for the CLI `install.sh`. This fixes the occasional 403 errors from the use of the action.

## [0.1.1] - 2025-03-27

* setup-cli: Add support for 'version' argument to specify the CLI version ('0.0.0' is special for head of main branch).

## [0.1.0] - 2025-02-09

* Added the `setup-cli' composite action which uses the new Metaplay CLI compatible with Release 32 and beyond.

## [0.0.4] - 2024-08-29

* Added the `setup-metaplay` composite action which can be used to get more control over the steps in your Github Action workflows.

## [0.0.3] - 2024-04-24

### Changed

* Use the new `metaplay-auth` commands `push-docker-image` and `deploy-server` in the build and deploy steps.

## [0.0.2] - 2024-03-20

### Changed

* Simplify the steps using the new `metaplay-auth get-docker-login`
* Bump default Helm version to v3.14.3.

## [0.0.1] - 2024-03-12

### Added

* Initial version of the reusable workflows for Github Actions.
* Reusable workflows for building and deploying the game server.
