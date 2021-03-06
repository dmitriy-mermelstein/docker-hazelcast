# Hazelcast
Hazelcast with clustering configuration based off the official Hazelcast Docker image. Uses 
multicast to automatically form a cluster. 

## Environment Variables
The default parameters can be overridden by setting environment variables on the container using the **docker run -e** flag.

 * **HZ_PORT** - Port to bind to.
 * **HZ_PUBLIC_HOST** - Public hostname to advertise to the cluster.
 * **HZ_PUBLIC_PORT** - Public port to advertise to the cluster. Defaults to $HZ_PORT
 * **HZ_GROUP_NAME** - Group name to avoid different clusters from joining each other.
 * **HZ_GROUP_PASSWORD** - Password required to join the cluster.

## Deployment
Deployment and clustering config examples.

### Marathon

```
{
  "id": "/myproduct/mysubsystem/hazelcast",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "meltwater/hazelcast:latest"
    }
  },
  "ports": [
    5701
  ],
  "env": {
    "HZ_PORT": "${PORT_5701}",
    "HZ_PUBLIC_HOST": "${HOST}",
    "HZ_GROUP_NAME": "mygroup",
    "HZ_GROUP_PASSWORD": "secret"
  },
  "instances": 3,
  "healthChecks": [
    {
      "protocol": "TCP",
      "portIndex": 0,
      "gracePeriodSeconds": 15,
      "intervalSeconds": 10,
      "timeoutSeconds": 20,
      "maxConsecutiveFailures": 3
    }
  ]
}
```

### Puppet Hiera

Using the [garethr-docker](https://github.com/garethr/garethr-docker) module

```
classes:
  - docker::run_instance

docker::run_instance:
  'hazelcast':
    image: 'meltwater/hazelcast:latest'
    net: 'host'
    env:
      - "HZ_GROUP_NAME=mygroup"
      - "HZ_GROUP_PASSWORD=secret"
```

### Command Line
```
docker run --net=host \
  -e HZ_GROUP_NAME=mygroup \
  -e HZ_GROUP_PASSWORD=secret \
  meltwater/hazelcast:latest
```
