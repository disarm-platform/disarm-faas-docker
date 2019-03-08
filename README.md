# DiSARM OpenFaas server configuration

Basic config for traefik and OpenFaas: [wiki](https://github.com/disarm-platform/disarm-faas-docker/wiki) has some instructions

## Quickly restore many functions

Ideally before anything goes down(!), get a copy of the `/functions` JSON response from the OpenFaas server. Save it e.g. `functions.json`. Then, using [`jq`](https://stedolan.github.io/jq/), [`httpie`](https://httpie.org) and [`parallel`](https://www.gnu.org/software/parallel/), run:

```bash
jq -r ".[] | .name,.image" < functions.json | parallel -N 2 http --ignore-stdin -a user:password https://faas.srv.disarm.io/system/functions service={1} image={2}
```



## Changing password

To change password, need to either down/up the stack, or replace individually for these services:

- `func_gateway`
- `func_faas-swarm`
- `fun_queue-worker`
- `func_alertmanager`


## Optional: logging on Google Cloud platform

Install agent:
- `curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh; sudo systemctl restart docker`

Set Docker to use it for all containers: 
- `echo '{"log-driver":"gcplogs"}' | sudo tee /etc/docker/daemon.json; sudo systemctl restart docker`

Resources: [from Google here](https://cloud.google.com/community/tutorials/docker-gcplogs-driver) and [from Docker here](https://docs.docker.com/config/containers/logging/gcplogs/)
