# Lesson 1 – Grafana Architecture Overview

## Introduction

Before working with dashboards and queries, it is important to understand **how Grafana is built internally**.

Grafana is not just a visualization tool.
It is a system composed of multiple layers, each with a clear responsibility.

Understanding Grafana’s architecture helps you:
- Avoid treating Grafana as a black box
- Design better dashboards and alerts
- Understand limitations and performance behavior
- Make correct architectural decisions

---

## High-Level Architecture

Grafana is composed of the following main components:

- Backend
- Frontend
- Datasource Layer
- Internal Storage (Configuration DB)

Important note:
> Grafana **does not store monitoring data**.  
> It only visualizes and analyzes data collected by other systems.

---

## Grafana Backend

The backend is the **core of Grafana**.
It is primarily written in **Go** and handles all server-side logic.

### Main Responsibilities

#### Authentication and Authorization
- User login and session handling
- Role-Based Access Control (RBAC)
- Organization-level isolation

#### Datasource Management
- Storing datasource configurations
- Managing credentials and authentication
- Routing queries to the correct datasource

#### Query Execution
- Receiving queries from the frontend
- Validating queries
- Sending queries to datasources
- Returning results to the frontend

If the backend is down, dashboards may load but **no data will appear**.

#### Alerting Engine
- Executes alert queries on the server side
- Evaluates thresholds and conditions
- Manages alert states (OK / Pending / Firing)
- Sends notifications

#### Internal Database Access
- Reads and writes Grafana configuration data
- Manages dashboards, users, folders, and alert rules

---

## Grafana Frontend

The frontend is what users interact with directly.
It is built using **React** and focuses entirely on presentation and interaction.

### Key Responsibilities

- Rendering dashboards and panels
- Managing user interactions
- Building queries based on UI actions
- Displaying data returned from the backend

The frontend contains **almost no monitoring logic**.
All real work is done by the backend.

### Core UI Elements

#### Dashboards
Dashboards define:
- Layout
- Panels
- Queries
- Variables
- Time ranges

They describe *what to ask* and *how to display it*.

#### Panels
Panels are visual representations such as:
- Time series
- Stat
- Table
- Heatmap

Panels do not fetch data directly.

#### Visualization Settings
- Colors
- Units
- Thresholds
- Legends
- Tooltips

All handled on the frontend.

---

## Internal Database

Grafana uses an internal database for **configuration only**.

Supported databases:
- SQLite (default)
- MySQL
- PostgreSQL

### Stored Data
- Users
- Roles
- Organizations
- Dashboards
- Folders
- Datasources
- Alert rules

### Not Stored
- Metrics
- Logs
- Traces

All observability data remains in the original datasource.

---

## Users and Roles

Grafana uses **Role-Based Access Control**.

### Default Roles

- **Admin**
  - Full system access
  - Manage users and datasources

- **Editor**
  - Create and edit dashboards
  - Write queries

- **Viewer**
  - Read-only access

Newer versions support fine-grained RBAC.

---

## Organizations

Grafana supports multiple **Organizations**.

Each organization has:
- Its own users
- Its own datasources
- Its own dashboards

This is useful for:
- Large companies
- Multi-team environments
- SaaS platforms

---

## Plugin System

Grafana is highly extensible through plugins.

### Plugin Types

#### Datasource Plugins
Used to connect to new data sources:
- Prometheus
- Loki
- Elasticsearch
- Cloud providers

#### Panel Plugins
Used to add new visualization types:
- Custom charts
- Diagrams
- Specialized gauges

#### App Plugins
Full-feature plugins that may include:
- Dashboards
- Panels
- Configuration pages

Commonly used for Kubernetes and cloud integrations.

---

## Summary

Key takeaways from this lesson:

- Grafana is not just a UI
- The backend is the brain of the system
- The frontend is responsible only for visualization
- Grafana does not store monitoring data
- Organizations and roles define security boundaries
- Plugins are the reason Grafana is powerful and flexible

Understanding this architecture is essential before building dashboards and alerts.

