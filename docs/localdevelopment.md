# Local Development

To start working with this project and launch a fully functional version of the service, please see [the following documentation]().

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

local.env file should be modified the following way:

```
ENVIRONMENT=local
TITLE=Exchange Rate Service (Local)
DESCRIPTION=The Exchange Rate service can be used to provide current and historic currency exchange rates.
NAME=Common Business Services
EMAIL=common-business-services@wexinc.com
CONTACT_URL=https://sites.google.com/wexinc.com/common-business-services/home
SERVICE_ROLE=exchange-rates:developer
RUN_GHERKIN_TESTS=False
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

## GraphQL Playground

This should allow you to connect to localhost (http:)
