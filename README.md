# DiSARM OpenFaas server configuration

Basic config for [OpenFaas](https://docs.openfaas.com/), [traefik](https://docs.traefik.io/) and [Portainer](https://portainer.readthedocs.io): instructions are in the [wiki](https://github.com/disarm-platform/disarm-faas-docker/wiki)

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

## Optional: monitoring on Google Cloud platform

Install agent:
- `curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh; sudo bash install-monitoring-agent.sh`


## Optional: logging on Google Cloud platform

Install agent:
- `curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh; sudo bash install-logging-agent.sh`

Set Docker to use it for all containers: 
- `echo '{"log-driver":"gcplogs"}' | sudo tee /etc/docker/daemon.json; sudo systemctl restart docker`

Resources: [from Google here](https://cloud.google.com/community/tutorials/docker-gcplogs-driver) and [from Docker here](https://docs.docker.com/config/containers/logging/gcplogs/)

## Set cron job to prune unused docker files
- `crontab -e`
- to run the command everyday at 4am insert `0 4 * * * /usr/bin/docker system prune -f`
