# Actions

We've moved this project over to github actions.  It's not fully enabled and we're still working to sort out bugs.

1) Code Coverage Reporting
---
**NOTE**

Because of Docker in Docker execution, I've been unable to generate coverage reports and add them to comments or build artifacts.
It's unclear what this might look like.

---

2) CD into Fargate

3) Change run script to pull artifacts from github instead of DTR

4) ~~Create a single github action repository for CBS so that upgrading across projects becomes easier~~
    This isn't possible, see: https://github.community/t5/GitHub-Actions/Being-DRY-centralized-workflows/td-p/34485

5) Linting
  * What linting will we enforce?
  * Dockerfile Linting
  * Shell Linting

6) Trigger Regression scripts on deployment to dev
    * Jira creation on failure

    ---
