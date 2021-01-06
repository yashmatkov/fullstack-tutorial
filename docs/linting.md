## Linting

Linting helps conform the coding to standards the team has adopted. We're currently trying the following linting tools:

  * [Pre-Commit Hooks](https://github.com/pre-commit/pre-commit-hooks)
  * [Shell-Lint](git://github.com/detailyang/pre-commit-shell)
  * [go fmt](https://github.com/dnephin/pre-commit-golang)
  * [go lint](https://github.com/dnephin/pre-commit-golang)
  * [go vet](https://github.com/dnephin/pre-commit-golang)
  * [golangci-lint](https://github.com/dnephin/pre-commit-golang)

To invoke the linters without installing them in your local environment run:
```bash
make lint
```

While its recommended that we work within containers, to invoke the linters natively on your mac, use:

```bash
make mac-lint
```

---
