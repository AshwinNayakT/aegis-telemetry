# AEGIS-1 System Architecture

## Project Overview

AEGIS-1 is a cloud-native security telemetry and detection platform designed to collect, process, analyze, and investigate security events.

The platform processes telemetry from multiple sources and transforms raw events into actionable security alerts.

---

# Architecture Overview

The AEGIS-1 security pipeline consists of the following stages:

Telemetry Sources
        ↓
Secure Ingestion API
        ↓
Validation
        ↓
Normalization
        ↓
Enrichment
        ↓
Detection Engine
        ↓
Storage and Analytics
        ↓
Security Alerts
        ↓
AI-Assisted Investigation

---

# 1. Telemetry Sources

Telemetry sources generate security-related events.

Examples include:

- Application logs
- Authentication events
- API activity
- Azure infrastructure events
- Security events
- Simulated attack telemetry

Telemetry may initially be generated using custom Python generators and later expanded to include Azure-native telemetry.

---

# 2. Secure Ingestion API

The ingestion API acts as the primary entry point for telemetry entering the AEGIS platform.

Responsibilities include:

- Receiving telemetry events
- Authenticating telemetry sources
- Request validation
- Request size restrictions
- Rate limiting
- Rejecting malformed events
- Recording ingestion activity

The ingestion layer represents an important trust boundary because telemetry may originate from untrusted or external systems.

---

# 3. Validation Layer

The validation layer ensures incoming telemetry conforms to the expected AEGIS event contract.

Responsibilities include:

- Schema validation
- Required field validation
- Data type validation
- Timestamp validation
- Invalid event rejection

Invalid telemetry must not enter the processing pipeline.

---

# 4. Normalization Layer

Different telemetry sources may use different event formats.

The normalization layer converts these events into a common AEGIS Security Event Schema.

Example:

Raw Event:

{
  "user": "example-user",
  "event": "login_failed",
  "ip": "203.0.113.10"
}

Normalized Event:

{
  "event_type": "authentication.failure",
  "actor": {
    "username": "example-user"
  },
  "source": {
    "ip": "203.0.113.10"
  }
}

---

# 5. Enrichment Layer

The enrichment layer adds additional security context to normalized telemetry.

Potential enrichment includes:

- IP context
- Geographic information
- Risk indicators
- Historical activity
- Asset context

Enrichment improves the quality of detection and investigation.

---

# 6. Detection Engine

The detection engine analyzes processed telemetry and identifies suspicious behavior.

Initial detection capabilities include:

- Brute-force detection
- Repeated authentication failures
- Suspicious privilege changes
- Abnormal API activity

Future detection capabilities may include:

- Event correlation
- Behavioral analysis
- Risk scoring
- Advanced anomaly detection

---

# 7. Storage and Analytics

AEGIS stores different stages of security telemetry.

Data categories include:

- Raw events
- Validated events
- Normalized events
- Enriched events
- Security alerts

Azure storage and analytics services will be selected during later implementation phases.

---

# 8. AI-Assisted Investigation

The AI investigation layer assists security analysts by analyzing alerts and providing contextual explanations.

Potential capabilities include:

- Alert summarization
- Attack explanation
- Risk assessment
- Investigation guidance
- Recommended response actions

AI-generated output will be treated as assistance for analysts and not automatically trusted as a security decision.

---

# Security Principles

AEGIS-1 follows these principles:

- Zero Trust
- Least Privilege
- Defense in Depth
- Secure by Default
- Data Minimization
- Observability
- Separation of Trust Boundaries

---

# High-Level Architecture

```text
TELEMETRY SOURCES
        │
        ▼
SECURE INGESTION API
        │
        ▼
VALIDATION
        │
        ▼
NORMALIZATION
        │
        ▼
ENRICHMENT
        │
        ▼
DETECTION ENGINE
        │
        ├──────────────► SECURITY STORAGE
        │
        ▼
SECURITY ALERTS
        │
        ▼
AI-ASSISTED INVESTIGATION
