![Banner](.attachments/prometheus-banner.png)
# Prometheus
![icon](.attachments/prometheus-icon.png) </br>

</br>
Prometheus, a Cloud Native Computing Foundation project, is a systems and service monitoring system. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed.
</br>
</br>

# ![icon](.attachments/d.png)/![icon](.attachments/k.png) Deployments

</br>

| Deployment                | Location           | Tags                        | Status       | Url                                  |
| ------------------------- | ------------------ | --------------------------- | ------------ | ------------------------------------ |
| prometheus-dev-1-vm-1     | dev-1-vm           | ![icon](.attachments/d.png) | **active**   | http://192.168.40.111:9090 — central scraper (all hosts) |
| prometheus-dev-2-vm-1     | dev-2-vm           | ![icon](.attachments/d.png) | **active**   | http://192.168.40.112:9090 — remote-write receiver (30d) |

# References
- ![icon](.attachments/github-icon.png)  [Github mirror](https://github.com/margusmuru/homelab-prometheus)
- ![icon](.attachments/github-icon.png)  [Prometheus Github](https://github.com/prometheus/prometheus)
- ![icon](.attachments/url-icon.png)   https://prometheus.io/



# Extension components

## node-exporter
![icon](.attachments/prometheus-icon.png) </br>

</br>
Node exporter is an official [Prometheus exporter](https://prometheus.io/docs/instrumenting/exporters/?ref=devopscube.com) for capturing all the [Linux](https://devopscube.com/list-linux-networking-troubleshooting-and-commands-beginners/) system-related metrics.
It collects all the hardware and Operating System level metrics that are exposed by the kernel.
You can use the node exporter to collect the system metrics from all your Linux systems. Check this article on [node monitoring using node-exporter](https://devopscube.com/monitor-linux-servers-prometheus-node-exporter/).
</br>
</br>

## ![icon](.attachments/d.png)/![icon](.attachments/k.png) Deployments

</br>

| Deployment                   | Location           | Tags                        | Status     | Url                                                              |
| ---------------------------- | ------------------ | --------------------------- | ---------- | ---------------------------------------------------------------- |
| node-exporter-portainer-vm-1 | portainer-vm       | ![icon](.attachments/d.png) | **active** | http://192.168.50.102:9100 |
| node-exporter-cloudy-vm-1    | cloudy-vm          | ![icon](.attachments/d.png) | **active** | http://192.168.40.102:9100 |
| node-exporter-dev-1-vm-1     | dev-1-vm           | ![icon](.attachments/d.png) | **active** | http://192.168.40.111:9100 |
| node-exporter-dev-2-vm-1     | dev-2-vm           | ![icon](.attachments/d.png) | **active** | http://192.168.40.112:9100 |
| node-exporter-prod-1-vm-1    | prod-1-vm          | ![icon](.attachments/d.png) | **active** | http://192.168.40.121:9100 |
| node-exporter-valhalla-vm-1  | valhalla-vm        | ![icon](.attachments/d.png) | **active** | http://192.168.50.10:9100 |

## Configuration
`deployments/node-exporter/docker-compose.yaml`:
- `network_mode: host` and `pid: host`: network and process metrics are the host's own. With a bridge network node-exporter only sees its own container interface, so host traffic was missing.
- Port 9100 is the host's port, not published by Docker, so it goes through the host firewall. Hosts with ufw (cloudy, portainer) allow it from prometheus on dev-1 only (`ufw_allow` in the linux-hosts Ansible repo). dev-1 scrapes it as `dev-1-vm.margusm.dev:9100`.
- Log rotation: 10 MB x 3.
- Footprint: ~0% CPU, ~10 MB RAM per host.

## References
- ![icon](.attachments/github-icon.png)  [node-exporter Github](https://github.com/prometheus/node_exporter)

## cAdvisor
![icon](.attachments/cadvisor-icon.png) </br>

</br>
cAdvisor (Container Advisor) provides container users an understanding of the resource usage and performance characteristics of their running containers. It is a running daemon that collects, aggregates, processes, and exports information about running containers. 
</br>
</br>

## ![icon](.attachments/d.png)/![icon](.attachments/k.png) Deployments

</br>

| Deployment               | Location           | Tags                        | Status     | Url                                                              |
| ------------------------ | ------------------ | --------------------------- | ---------- | ---------------------------------------------------------------- |
| cadvisor-portainer-vm-1  | portainer-vm       | ![icon](.attachments/d.png) | **active** | http://192.168.50.102:8081 |
| cadvisor-cloudy-vm-1     | cloudy-vm          | ![icon](.attachments/d.png) | **active** | http://192.168.40.102:8081 |
| cadvisor-dev-1-vm-1      | dev-1-vm           | ![icon](.attachments/d.png) | **active** | http://192.168.40.111:8081 |
| cadvisor-dev-2-vm-1      | dev-2-vm           | ![icon](.attachments/d.png) | **active** | http://192.168.40.112:8081 |
| cadvisor-prod-1-vm-1     | prod-1-vm          | ![icon](.attachments/d.png) | **active** | http://192.168.40.121:8081 |
| cadvisor-valhalla-vm-1   | valhalla-vm        | ![icon](.attachments/d.png) | **active** | http://192.168.50.10:8081 |

## Configuration
`deployments/cadvisor/docker-compose.yaml` trims cAdvisor's defaults, which cost 5-14% of a core and 80 MB-1.3 GB RAM per host:

| Setting | Why |
| --- | --- |
| `--housekeeping_interval=15s` | Default is 1s; 15s matches the Prometheus scrape interval |
| `--docker_only=true` | Only Docker containers plus host-level totals (`id="/"`), no systemd slices |
| `--store_container_labels=false` | Docker labels (compose hashes, OCI metadata) are not copied onto every series; `name` and `image` labels stay |
| `--disable_metrics=...,disk,...` | `disk` is the per-container du scan (`container_fs_usage_bytes`), the most expensive collector. `diskIO` (`container_fs_reads/writes_bytes_total`) stays. Also off: advtcp, cpu_topology, cpuset, hugetlb, memory_numa, percpu, process, referenced_memory, resctrl, sched, tcp, udp |
| `mem_limit: 512m` | Cap in case it grows again |
| Log rotation 10 MB x 3 | |

Result (measured 2026-10-17): **0.15-0.3% CPU, ~20 MB RAM** per host.

Host network traffic for the Grafana "Hosts overview" comes from cAdvisor's host-level series: `container_network_*{id="/", interface=~"ens.*"}`.

`CADVISOR_DOCKER_LIB` (optional CI/CD variable) overrides the Docker root mounted into cAdvisor; default `/var/lib/docker`.

## References
- ![icon](.attachments/github-icon.png)  [cAdvisor Github](https://github.com/google/cadvisor)


