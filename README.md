![Banner](.attachments/prometheus-banner.png)
# Prometheus
![icon](.attachments/prometheus-icon.png) </br>

</br>
Prometheus, a Cloud Native Computing Foundation project, is a systems and service monitoring system. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed.
</br>
</br>

# ![icon](.attachments/d.png)/![icon](.attachments/k.png) Deployments

</br>

| Deployment                | Location           | Tags                        | Status       | Url                                                                         |
| ------------------------- | ------------------ | --------------------------- | ------------ | --------------------------------------------------------------------------- |
| prometheus-portainer-vm-1 | portainer-vm       | ![icon](.attachments/d.png) | **inactive** | https://prometheus-portainer.margusm.dev/<br>http://192.168.50.102:9090<br> |
| prometheus-cloudy-vm-1    | cloudy-vm          | ![icon](.attachments/d.png) | **inactive** | https://prometheus-cloudy.margusm.dev/<br>http://192.168.40.102:9090<br>    |
| prometheus-dev-1-vm-1     | dev-1-vm           | ![icon](.attachments/d.png) | **active**   | http://192.168.40.111:9090<br>  
| prometheus-dev-2-vm-1     | dev-2-vm           | ![icon](.attachments/d.png) | **inactive** | http://192.168.40.112:9090<br>                                              |
| prometheus-dev-2-vm-2     | dev-2-vm           | ![icon](.attachments/d.png) | **inactive** | http://192.168.40.112:9091<br>http://prometheus-dev-2.margusm.dev           |

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
| node-exporter-dev-2-vm-2     | dev-2-vm           | ![icon](.attachments/d.png) | **active** | http://192.168.40.112:9100 |

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
| cadvisor-dev-2-vm-2      | dev-2-vm           | ![icon](.attachments/d.png) | **active** | http://192.168.40.112:8081 |

## References
- ![icon](.attachments/github-icon.png)  [cAdvisor Github](https://github.com/google/cadvisor)


