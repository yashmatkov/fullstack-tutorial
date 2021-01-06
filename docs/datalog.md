# Datadog
  A Datadog agent runs in a separate container during integration testing. The agent collects the service container's metrics from docker and send them to the Datadog SaaS.
  The configurations for the Datadog agent are in the "docker-compose-integration.yml" and the config/*.env files.
  For more details on agent config, see: https://docs.datadoghq.com/agent/guide/autodiscovery-management/?tab=containerizedagent.
  You can review the metrics in the SaaS by filtering hosts using the value set for DD_HOSTNAME.

## Enabling DD Tracing Locally

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

---
