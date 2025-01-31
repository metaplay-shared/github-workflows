# Changelog

## [0.0.5] - Unpublished

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
