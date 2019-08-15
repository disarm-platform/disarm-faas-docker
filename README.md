# DiSARM OpenFaas server configuration

Basic config for [OpenFaas](https://docs.openfaas.com/), [traefik](https://docs.traefik.io/) and [Portainer](https://portainer.readthedocs.io): instructions are in the [wiki](https://github.com/disarm-platform/disarm-faas-docker/wiki)


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
