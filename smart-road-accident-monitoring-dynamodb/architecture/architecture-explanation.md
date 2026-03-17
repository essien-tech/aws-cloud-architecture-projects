# Architecture Explanation

## Overview

This architecture demonstrates a **real-time road accident monitoring system** powered by Amazon DynamoDB.

It is designed to enable:

- fast ingestion of accident reports
- real-time querying of incidents
- scalable data storage for large datasets

---

## Architecture Components

### 1. Data Ingestion Layer

Accident data can come from:

- mobile applications (drivers, citizens)
- traffic authorities
- IoT sensors (future extension)

These inputs send structured data into the system.

---

### 2. Amazon DynamoDB (Core Database)

DynamoDB acts as the central data store.

Key capabilities:
- fully managed NoSQL database
- single-digit millisecond latency
- automatic scaling
- high availability

---

### 3. Global Secondary Index (GSI)

A GSI is created to enable efficient querying.

**Primary Table Access:**
- Query by `IncidentID`

**GSI Access:**
- Query by `IncidentDate`
- Sort by `SeverityLevel`

This enables:

- quick filtering of recent incidents
- prioritization of critical accidents

---

### 4. Query & Access Layer

Emergency responders and dashboards can:

- fetch recent accidents
- filter by severity
- monitor trends

---

## Data Flow

1. Incident occurs
2. Data is submitted into DynamoDB
3. GSI indexes the data
4. Responders query:
   - latest incidents
   - high-severity cases
5. Faster response decisions are made

---

## Key Architecture Benefits

- Real-time performance
- High scalability
- Fault tolerance
- Low operational overhead
