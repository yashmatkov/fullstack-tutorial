# Exchange Rate Service

![Pull Request](https://github.com/wexinc/ps-cbs-exchange-rate-service/workflows/Pull%20Request/badge.svg)

Various teams across WEX require the use of exchange rates. This includes current and historical rates.   <Todo, statement on what processes use the exchange rates>

An example of an external service is as follows: (https://openexchangerates.org/)

## Technical Overview

### Getting Started
For anything related to contributing code, submitting issues, etc.  Please see the [Contributing Guidelines](docs/CONTRIBUTING.md).

### Development

### Pre-requisites
You'll need a minimal amount of supporting libraries to get started (and assumes MacOS X). This includes:

* Docker
* Docker-compose
* awscli
* xcode
* brew
* python3

### Local Development
To start working with this project and launch a fully functional version of the service, please see [the following documentation](localdevelopment.md).

---

### Linting
Linting helps conform the coding to standards the team has adopted. We're currently trying the following linting tools:

  * [Pre-Commit Hooks](https://github.com/pre-commit/pre-commit-hooks)
  * [Shell-Lint](git://github.com/detailyang/pre-commit-shell)
  * [go fmt](https://github.com/dnephin/pre-commit-golang)
  * [go lint](https://github.com/dnephin/pre-commit-golang)
  * [go vet](https://github.com/dnephin/pre-commit-golang)
  * [golangci-lint](https://github.com/dnephin/pre-commit-golang)

Here is the [instructions](linting.md) how to invoke the linters without installing them in your local environment.



### Testing

You can check the testing tools [here](testing.md).


### Workflows
Two workflows are used to manage CI for this project.

#### Pull Request
When a pull request is run, we lint, unit test, and then run the localized integration tests.  This verifies everything that a developer would run locally.

#### Merge

[The merge process](merge.md)


### Setting up your go environment

[Setting up your go environment](goenvironment.md)


#### More information
For more information about code organization and local environments, see the following:

* https://golang.org/doc/code.html#Organization
* https://www.digitalocean.com/community/tutorials/how-to-install-go-and-set-up-a-local-programming-environment-on-macos
* https://medium.com/@radlinskii/writing-the-pre-commit-git-hook-for-go-files-810f8d5f1c6f



## Deployment
The notification-service is packaged into docker containers.  The containers are built manually (commands TBD) and
pushed up to DTR.
Future iterations will be built via CI.

Please see [this documentation](deployment.md) for details.

## Logging
Logging can be found in Sumologic (SaaS).  If you don't have access to Sumo, please feel free to open a Cherwell ticket
requesting okta access to it.

Logs are gathered from the container output. The following project was referenced for how to enable the logging container: https://github.com/SumoLogic/sumologic-collector-docker

## Actions
We've moved this project over to github actions.  It's not fully enabled and we're still working to sort out bugs.  [Here's some things we still need to work through:](actions.md)


## Datadog
  A Datadog agent runs in a separate container during integration testing. The agent collects the service container's metrics from docker and send them to the Datadog SaaS.
  The configurations for the Datadog agent are in the "docker-compose-integration.yml" and the config/*.env files.
  For more details on agent config, see: https://docs.datadoghq.com/agent/guide/autodiscovery-management/?tab=containerizedagent.
  You can review the metrics in the SaaS by filtering hosts using the value set for DD_HOSTNAME.

### Enabling DD Tracing Locally

  [Enabling DD Tracing Locally](ddtracing.md)
