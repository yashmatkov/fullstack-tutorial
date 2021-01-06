
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
