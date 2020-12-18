# Exchange Rate Service

![Pull Request](https://github.com/wexinc/ps-cbs-exchange-rate-service/workflows/Pull%20Request/badge.svg)

Various teams across WEX require the use of exchange rates. This includes current and historical rates.   <Todo, statement on what processes use the exchange rates>

An example of an external service is as follows: (https://openexchangerates.org/)

## Technical Overview

### Getting Started
For anything related to contributing code, submitting issues, etc.  Please see the [Contributing Guidelines](CONTRIBUTING.md).

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
To start working with this project and launch a fully functional version of the service, do the following:

Clone the repository:

```bash
git clone git@github.com:wexinc/ps-cbs-exchange-rate-service.git
```

Set the integration variables in the `ps-cbs-exchange-rate-service/config/.env` file:

---
**NOTE**

The .env file has been added to the .gitignore file. Changes made to this file will not
be committed to the repository.

---

```.env
FIXERIO_API_KEY=
```

Start the service and supporting services:

```bash
make bootstrap
```

---
**NOTE**

This will terminate any running containers related to the project, build new containers, and then start them. It also
loads some sample data to work with.  You can then make calls against the service using the client script.

---

### Linting
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

### Unit Testing
Unit tests are used to ensure proper operation of functional areas within the code.  The unit tests shouldn't rely on
any environment variables.

```bash
make test
```

### Localized Integration Testing
Localized integration testing is run to verify the depth and breadth of the application.

```
make integration
```

---
**NOTE**

You'll need to set any third party credentials to fully exercise the localized integration tests.
You'll see an error similar to the following: "FixerIO vendor API key unset. Please check 1Password for it. Exiting now."
In order to solve this error, you need to go to the secret manager in the AWS console, get the value of the key:

```
exchange.fixerio_api_key
```

And add its value on the variable below, inside the local.env file:

```
FIXERIO_API_KEY=<exchange.fixerio_api_key value>
```

### Regression Testing
Regression testing is configured to be applied against our development environment.

```
make regression
```

---
**NOTE**

You'll need to set any third party credentials to fully exercise the localized integration tests.
You'll see an error similar to the following: "FixerIO vendor API key unset. Please check 1Password for it. Exiting now."

---

### Workflows
Two workflows are used to manage CI for this project.

#### Pull Request
When a pull request is run, we lint, unit test, and then run the localized integration tests.  This verifies everything that a developer would run locally.

#### Merge
When a pull request is merged into master, we run all of the steps we'd run on a pull request.  If these all succeed, a
new Semver formatted version tag is applied to the master branch.  Any commit message that includes `#major`, `#minor`,
or `#patch` will trigger the respective version bump.  If two or more are present, the highest-ranking one will take
precedence. A new changelog is then built and published on master.

### Setting up your go environment

#### Do Once
Setup your golang environment by setting the following in your ~/.bash_profile or ~/.bashrc
```bash
export GOPATH=$HOME/go # don't forget to change your path correctly!
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
```

Setup your the workspace directory tree:
```bash
mkdir -p $GOPATH $GOPATH/src $GOPATH/pkg $GOPATH/bin
mkdir -p $GOPATH/src/github.com/wexinc
```

Clone the service
```bash
cd $GOPATH/src/github.com/wexinc/
git clone git@github.com:wexinc/ps-cbs-notification_service.git
```

Setup pre-commit hooks: [`goimports`](https://godoc.org/golang.org/x/tools/cmd/goimports), [`go lint`](https://github.com/golang/lint), [`go vet`](https://godoc.org/github.com/golang/go/src/cmd/vet)
```bash
go get golang.org/x/tools/cmd/goimports
go get -u golang.org/x/lint/golint
cp $GOPATH/src/github.com/wexinc/ps-cbs-notification_service/script/hooks/pre-commit $GOPATH/src/github.com/wexinc/ps-cbs-notification_service/.git/hooks/pre-commit
```

Setup your local git environment to use ssh instead of HTTPS

Add the following lines to your `~/.gitconfig` file. Create it if it doesn't exist.
```bash
[url "ssh://git@github.com/"]
	insteadOf = https://github.com/
```

#### Iterate
Add import statements to .go code as needed. Build the binary.
```bash
cd $GOPATH/src/github.com/wexinc/<project>

# avoid external lookups of our private modules
export GOPRIVATE="github.com/wexinc"

go build
```

#### go mod dependency management
This has already been done, but I thought I'd document it.  Go build does `go get` for you now.
```bash
cd $GOPATH/src/github.com/wexinc/notification_service
go mod init github.com/wexinc/notification_service
```

#### More information
For more information about code organization and local environments, see the following:

* https://golang.org/doc/code.html#Organization
* https://www.digitalocean.com/community/tutorials/how-to-install-go-and-set-up-a-local-programming-environment-on-macos
* https://medium.com/@radlinskii/writing-the-pre-commit-git-hook-for-go-files-810f8d5f1c6f



## Deployment
The notification-service is packaged into docker containers.  The containers are built manually (commands TBD) and
pushed up to DTR.
Future iterations will be built via CI.

Build a version of the container locally
```bash
docker build -t dtr.wexapps.com/notification-service-web:v0.0.1 .
```

Once the containers are in DTR, a deployment script can be run on the host.  SSH over to the host and then use the
`run.sh` script. This will pull the container to the host, stop any old containers, and start the new ones.

## Logging
Logging can be found in Sumologic (SaaS).  If you don't have access to Sumo, please feel free to open a Cherwell ticket
requesting okta access to it.

Logs are gathered from the container output. The following project was referenced for how to enable the logging container: https://github.com/SumoLogic/sumologic-collector-docker

## Actions
We've moved this project over to github actions.  It's not fully enabled and we're still working to sort out bugs.  Here's some things we still need to work through:

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

## Datadog
  A Datadog agent runs in a separate container during integration testing. The agent collects the service container's metrics from docker and send them to the Datadog SaaS.
  The configurations for the Datadog agent are in the "docker-compose-integration.yml" and the config/*.env files.
  For more details on agent config, see: https://docs.datadoghq.com/agent/guide/autodiscovery-management/?tab=containerizedagent.
  You can review the metrics in the SaaS by filtering hosts using the value set for DD_HOSTNAME.

  ### Enabling DD Tracing Locally
  Add the following image in the relevant docker-compose file. Make sure to have the DD_API_KEY variable defined within "config/local.env".
  ```
    datadog-agent:
    image: datadog/agent:7
    ports:
      - "8126/tcp"
    networks:
      - {{ main service's network }}
    env_file:
      - ./config/local.env
    environment:
      - DD_APM_ENABLED=true
      - DD_APM_NON_LOCAL_TRAFFIC=true
      - DD_LOGS_ENABLED=false
      - DD_TAGS="org:ps group:ea team:cbs app:exchange_service event:trace env:local"
    volumes:
      - '/var/run/docker.sock:/var/run/docker.sock:ro'
      - '/proc:/host/proc:ro'
      - '/sys/fs/cgroup/:/host/sys/fs/cgroup:ro'
  ```
