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
