# Workflows
Two workflows are used to manage CI for this project.

## Pull Request
When a pull request is run, we lint, unit test, and then run the localized integration tests.  This verifies everything that a developer would run locally.

## Merge
When a pull request is merged into master, we run all of the steps we'd run on a pull request.  If these all succeed, a
new Semver formatted version tag is applied to the master branch.  Any commit message that includes `#major`, `#minor`,
or `#patch` will trigger the respective version bump.  If two or more are present, the highest-ranking one will take
precedence. A new changelog is then built and published on master.

---
