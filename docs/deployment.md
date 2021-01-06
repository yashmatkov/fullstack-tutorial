# Deployment

The notification-service is packaged into docker containers.  The containers are built manually (commands TBD) and
pushed up to DTR.
Future iterations will be built via CI.

Build a version of the container locally
```bash
docker build -t dtr.wexapps.com/notification-service-web:v0.0.1 .
```

Once the containers are in DTR, a deployment script can be run on the host.  SSH over to the host and then use the
`run.sh` script. This will pull the container to the host, stop any old containers, and start the new ones.

---
