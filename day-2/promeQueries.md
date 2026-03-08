Here is the **README.md formatted version** so you can directly copy into your repo.

````markdown
# Production PromQL Queries for Kubernetes Monitoring

These are commonly used **production-level PromQL queries** used by DevOps / SRE engineers for monitoring Kubernetes clusters using **Prometheus and Grafana**.

---

# 1. Node CPU Usage %

Shows CPU utilization per node.

```promql
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
````

Use case:

* Detect high CPU nodes
* Capacity planning

---

# 2. Pod CPU Usage

Shows CPU usage per pod.

```promql
sum(rate(container_cpu_usage_seconds_total{container!="",image!=""}[5m])) by (pod)
```

Use case:

* Identify CPU-heavy pods
* Performance troubleshooting

---

# 3. Pod Memory Usage

Shows memory consumption per pod.

```promql
sum(container_memory_working_set_bytes{container!=""}) by (pod)
```

Use case:

* Detect memory-hog pods
* Memory leak analysis

---

# 4. Node Memory Usage %

```promql
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))
```

Use case:

* Monitor node memory pressure
* Trigger alerts when memory > 80%

---

# 5. Pod Restarts

Detect pods restarting frequently.

```promql
increase(kube_pod_container_status_restarts_total[10m])
```

Use case:

* Detect CrashLoopBackOff
* Debug failing containers

---

# 6. Pods in Pending State

```promql
kube_pod_status_phase{phase="Pending"}
```

Use case:

* Scheduling issues
* Insufficient cluster resources

---

# 7. Node Disk Usage %

```promql
100 - ((node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes)
```

Use case:

* Detect full disks
* Prevent node failures

---

# 8. Kubernetes API Server Latency

```promql
histogram_quantile(
  0.99,
  sum(rate(apiserver_request_duration_seconds_bucket[5m])) by (le)
)
```

Use case:

* Monitor API server performance
* Detect control plane latency

---

# 9. Network Traffic per Pod

```promql
sum(rate(container_network_receive_bytes_total[5m])) by (pod)
```

Use case:

* Detect abnormal traffic spikes
* Identify high network usage pods

---

# 10. Cluster CPU Utilization %

```promql
sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) 
/
sum(machine_cpu_cores) * 100
```

Use case:

* Overall cluster utilization
* Capacity planning

---

# 11. Kubernetes Node Not Ready

```promql
kube_node_status_condition{condition="Ready",status="true"} == 0
```

Use case:

* Detect unhealthy nodes

---

# 12. Pod OOM Kills

```promql
increase(container_oom_events_total[5m])
```

Use case:

* Identify memory limit issues

---

# 13. High CPU Alert (Production Alert)

```promql
sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (pod) > 0.8
```

Use case:

* Trigger alert when pod CPU > 80%

---

# 14. High Memory Alert

```promql
(container_memory_working_set_bytes / container_spec_memory_limit_bytes) * 100 > 80
```

Use case:

* Prevent OOM kills

---

# 15. Kubernetes Deployment Availability

```promql
kube_deployment_status_replicas_available
/
kube_deployment_spec_replicas * 100
```

Use case:

* Monitor deployment health

---

# Bonus Query

## Top 5 CPU Consuming Pods

```promql
topk(5, sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (pod))
```

Use case:

* Identify most resource-consuming workloads

---

# Monitoring Areas in Production

Senior DevOps engineers typically monitor:

| Area             | Metrics                  |
| ---------------- | ------------------------ |
| Node Health      | CPU, Memory, Disk        |
| Pod Health       | Restarts, Pending        |
| Cluster Capacity | CPU / Memory utilization |
| Network          | Traffic spikes           |
| Control Plane    | API server latency       |
| Storage          | Persistent Volume usage  |
| Workloads        | Deployment availability  |

---

# Tools Used

* Prometheus
* Grafana
* Kubernetes
* kube-state-metrics
* node-exporter

```

If you're building a **DevOps portfolio repo**, I can also give you a **complete `Kubernetes Monitoring README.md` (like senior engineers write)** including:

- Architecture diagram  
- Prometheus + Grafana setup  
- Helm installation  
- Storage (EBS)  
- Alertmanager rules  
- PromQL queries  
- Troubleshooting commands  

It will look **very strong on GitHub for DevOps interviews.**
```
