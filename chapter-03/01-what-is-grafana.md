# Lesson 1: Introduction to Grafana

## Goal of This Lesson
- Build the correct **mental model** of Grafana
- Understand **why Grafana exists**, not how to click it
- Prepare for practical dashboards in later lessons

> Grafana is not about charts.  
> Grafana is about **understanding system behavior**.

---

## Why We Start Here (Before Any Clicks)
- Many people start Grafana by:
  - Creating dashboards
  - Adding random panels
  - Copy-pasting queries
- Result:
  - Too many dashboards
  - Little real understanding
  - Panic during incidents

**Core idea:**  
If you don’t know *what question a panel answers*, the panel is useless.

---

## What Is Grafana?
- Open-source **visualization and analytics platform**
- Mainly focused on **time series data**
- Used to **observe, analyze, and correlate** data over time

### Time Series Data
Data that:
- Changes over time
- Has a timestamp
- Can show trends and patterns

Examples:
- CPU usage over 24 hours
- Requests per second
- API latency over a week
- Error rate per minute

---

## What Grafana Is NOT
- Grafana does **not**:
  - Collect metrics
  - Store logs
  - Generate traces

Grafana works **on top of existing data sources**.

> Prometheus collects  
> Loki stores logs  
> Tempo traces  
> **Grafana makes sense of all of them**

---

## Simple Definition
> **Grafana is the eyes of your system.**

---

## Why Grafana Was Created
### Old World
- Few servers
- Monolithic applications
- Simple failure patterns
- Monitoring = CPU + Disk + Ping

### Modern World
- Distributed systems
- Microservices
- Kubernetes
- Many dependencies
- One issue → many possible root causes

**Traditional monitoring fails here.**

---

## The Core Problem Grafana Solves
- Too much data, not enough understanding
- Metrics alone are not enough
- Logs alone are not enough
- Alerts alone are not enough

Grafana helps you:
- See everything **in one place**
- Understand **relationships between signals**
- Compare **before vs after incidents**
- Move from reactive to proactive operations

---

## What Can Grafana Visualize?

### Infrastructure Metrics
- CPU usage
- Memory usage
- Disk I/O
- Network traffic

### Service Metrics
- Service up/down
- Request rate
- Error rate
- Latency

### Application & Business Metrics
- User signups
- Successful payments
- Order processing time
- Queue length

### Logs & Traces
- Logs via Loki
- Traces via Tempo or Jaeger

**Key strength:** metrics, logs, and traces together.

---

## Grafana in the Monitoring Stack
Grafana is usually the **visualization layer**.

Common data sources:
- Prometheus (metrics)
- Loki (logs)
- Tempo / Jaeger (traces)
- Elasticsearch
- InfluxDB
- Zabbix

> Grafana is where all signals meet.

---

## Grafana vs Traditional Monitoring Tools

Traditional tools:
- Static dashboards
- Limited flexibility
- Alert-focused
- Tell you *what broke*

Grafana:
- Fully customizable
- Dynamic dashboards
- Multi–data source views
- Helps explain *why it broke*

---

## Real-World Scenario
**User says:** “The website is slow.”

Without Grafana:
- Guessing
- Manual log checks
- Trial and error

With Grafana:
- Check CPU & memory
- Analyze disk I/O
- Look at request latency
- Review error rates
- All in one dashboard

---

## Key Takeaways
- Grafana is not just charting
- Every panel must answer a question
- Dashboards without purpose are noise
- Grafana helps you understand systems, not just monitor them

---

## What’s Next
- Installing Grafana
- Connecting data sources
- Building **purpose-driven dashboards**

