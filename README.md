# cloud-native-5g-core

A learning project: an Open5GS 5G Core design on Docker Compose, built in GitHub Codespaces.

## Goal
Understand the 5G Core Service Based Architecture and practice Docker, Compose, log-reading and Git.

## Architecture
- **Database:** MongoDB (subscriber data)
- **Control plane:** NRF, UDR, UDM, AUSF, PCF, AMF, SMF
- **User plane:** UPF (`ogstun` via `/dev/net/tun`)

## Status
| Component | Status |
|---|---|
| MongoDB | Running |
| NRF | Running and verified |
| UDR | Blocked: cannot connect to MongoDB |
| UDM, AUSF, PCF, AMF, SMF, UPF | Config files and Compose services written, **untested** |
| UERANSIM, Terraform, CI | Not started |

## Troubleshooting log
1. `epoll_init failed` on Open5GS 2.7.2 in Codespaces: downgraded to 2.6.4.
2. `unknown key 'server'` and `TLS enabled but no server key`: config format did not match the version. Read the original config from inside the image and rebuilt it with `no_tls: true`.
3. `getaddrinfo(scp)`: removed the unused `scp` section.
4. UDR could not find NRF: added an `nrf` section using `name` and `port`.
5. UDR cannot reach MongoDB: still under investigation (even a plain `mongosh` test timed out).

## Run (verified part only)
```
cd docker
docker compose up -d mongodb nrf
docker compose logs nrf
```

## What I learned
Docker Compose, networks and volumes, reading container logs, systematic debugging, Git workflow.