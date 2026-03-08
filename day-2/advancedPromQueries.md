Below is a **README.md formatted list of advanced PromQL queries** commonly used by senior **Prometheus / Kubernetes / Grafana** DevOps engineers during **production incidents (P1 outages)**.

You can directly paste this into your repo.

````markdown
# Advanced PromQL Queries Used During Production Incidents (P1 Outages)

These queries help DevOps / SRE engineers quickly identify root causes during **production outages**, **performance degradation**, and **infrastructure failures**.

---

# 1. Top 10 CPU Consuming Pods

```promql
topk(10, sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (pod))
````

Use case:

* Identify pods consuming excessive CPU.

---

# 2. Top 10 Memory Consuming Pods

```promql
topk(10, sum(container_memory_working_set_bytes{container!=""}) by (pod))
```

Use case:

* Find pods causing memory pressure.

---

# 3. Pod CPU Usage vs CPU Limits %

```promql
sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (pod)
/
sum(container_spec_cpu_quota{container!=""}/container_spec_cpu_period{container!=""}) by (pod) * 100
```

Use case:

* Detect CPU throttling.

---

# 4. CPU Throttling Detection

```promql
sum(rate(container_cpu_cfs_throttled_seconds_total[5m])) by (pod)
```

Use case:

* Identify containers throttled by CPU limits.

---

# 5. Node CPU Saturation

```promql
sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (node)
/ sum(machine_cpu_cores) by (node) * 100
```

Use case:

* Detect overloaded nodes.

---

# 6. Memory Pressure Detection

```promql
(node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes) * 100
```

Use case:

* Identify nodes running out of memory.

---

# 7. Pods Causing Memory Leaks

```promql
increase(container_memory_working_set_bytes[30m])
```

Use case:

* Detect continuously increasing memory usage.

---

# 8. High Pod Restart Rate

```promql
topk(10, increase(kube_pod_container_status_restarts_total[15m]))
```

Use case:

* Identify unstable containers.

---

# 9. Detect CrashLoopBackOff Pods

```promql
kube_pod_container_status_waiting_reason{reason="CrashLoopBackOff"}
```

Use case:

* Find failing pods immediately.

---

# 10. Network Receive Traffic Spike

```promql
topk(10, sum(rate(container_network_receive_bytes_total[5m])) by (pod))
```

Use case:

* Detect abnormal traffic spikes.

---

# 11. Network Transmit Spike

```promql
topk(10, sum(rate(container_network_transmit_bytes_total[5m])) by (pod))
```

Use case:

* Identify pods sending excessive traffic.

---

# 12. Disk Usage per Node

```promql
100 - ((node_filesystem_avail_bytes * 100) / node_filesystem_size_bytes)
```

Use case:

* Detect nodes running out of disk.

---

# 13. Kubernetes API Server Latency (99th Percentile)

```promql
histogram_quantile(
  0.99,
  sum(rate(apiserver_request_duration_seconds_bucket[5m])) by (le)
)
```

Use case:

* Detect control plane performance issues.

---

# 14. API Server Error Rate

```promql
sum(rate(apiserver_request_total{code=~"5.."}[5m]))
/
sum(rate(apiserver_request_total[5m])) * 100
```

Use case:

* Detect API server failures.

---

# 15. Pending Pods Due to Scheduling Issues

```promql
sum(kube_pod_status_phase{phase="Pending"}) by (namespace)
```

Use case:

* Identify scheduling bottlenecks.

---

# 16. Deployment Availability %

```promql
kube_deployment_status_replicas_available
/
kube_deployment_spec_replicas * 100
```

Use case:

* Detect degraded deployments.

---

# 17. Kubernetes Node Not Ready

```promql
kube_node_status_condition{condition="Ready",status="true"} == 0
```

Use case:

* Detect failing nodes.

---

# 18. Detect OOMKilled Containers

```promql
increase(container_oom_events_total[10m])
```

Use case:

* Identify memory limit problems.

---

# 19. Top Namespaces Consuming CPU

```promql
topk(10,
sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (namespace)
)
```

Use case:

* Detect noisy namespaces.

---

# 20. Cluster CPU Utilization %

```promql
sum(rate(container_cpu_usage_seconds_total{container!=""}[5m]))
/
sum(machine_cpu_cores) * 100
```

Use case:

* Monitor overall cluster load.

---

# Typical P1 Incident Debugging Flow

During a production outage engineers usually check:

1. Node health (CPU, Memory, Disk)
2. Pod restarts
3. CrashLoopBackOff containers
4. Network spikes
5. API server latency
6. Deployment availability
7. OOMKilled containers

---

# Tools Used in Production Monitoring

* Prometheus
* Grafana
* Kubernetes
* kube-state-metrics
* node-exporter
* Alertmanager

```

---

✅ If you're building a **DevOps portfolio or GitHub monitoring repo for interviews**, I can also give you:

- **30 real production Alertmanager rules used by SRE teams**
- **10 P1 outage troubleshooting commands (kubectl + Prometheus)**
- **Complete Kubernetes Monitoring Architecture diagram**

These things make your **GitHub look like a Senior DevOps engineer’s project**, not a beginner one.
```
