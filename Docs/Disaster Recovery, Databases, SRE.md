# Disaster Recovery, Databases and SRE 

## Disaster Recovery
Disaster Recovery (DR) is the strategic process an organisation uses to regain access and functionality to its IT infrastructure after a disruptive event, such as a natural disaster, hardware failure, or cyberattack.

## RTO vs RPO
These are business continuity metrics used to define recovery expectations after a failure.

- **RTO (Recovery Time Objective):** The maximum tolerable downtime after a failure. Question: "How quickly must we be back online?" Focus: recovery speed and restoration of services.
- **RPO (Recovery Point Objective):** The maximum tolerable amount of data loss measured in time. Question: "How much data can we afford to lose?" Focus: frequency of backups; it looks back to the last valid recovery point.


## SQL vs NoSQL
This comparison defines how data is structured, stored, and scaled.

- **SQL (Relational):** Uses a fixed schema and stores data in predefined tables with rows and columns. It typically scales vertically by increasing CPU/RAM on a single server. It follows ACID properties to ensure high reliability for transactions. Best for complex queries and highly structured data like banking systems.
- **NoSQL (Non-Relational):** Uses dynamic schemas for unstructured, semi-structured, or polymorphic data such as documents, key-value pairs, and graphs. It scales horizontally by adding more commodity servers to a cluster. It often follows the CAP theorem, prioritizing availability and partition tolerance over immediate consistency. Ideal for big data, real-time web apps, and rapidly evolving data models.

| Aspect | SQL | NoSQL |
|---|---|---|
| Structure | Fixed schema, tables, rows, columns | Dynamic schema, documents, key-value, graphs |
| Scalability | Vertical | Horizontal |
| Consistency | ACID | Often CAP-oriented |
| Best Use | Structured transactions | Big data, real-time apps |

## SRE: SLA vs SLO vs SLI
This three-layer hierarchy is the backbone of Site Reliability Engineering for measuring and maintaining performance.

- **SLI (Service Level Indicator):** The actual measurement of performance. Example: current error rate is 0.01% or 95th percentile latency is 150 ms.
- **SLO (Service Level Objective):** The internal target set for an SLI over a specific period. Example: 99.9% of requests must succeed every 30 days.
- **SLA (Service Level Agreement):** The external contract with customers that defines consequences if SLOs are missed. Example: if uptime drops below 99.5%, the customer receives a 10% credit.

| Term | Scope | Target Audience | Consequence of Failure |
|---|---|---|---|
| SLI | Measurement | Engineering | Indicates technical issues |
| SLO | Target | Product/Engineering | Consumes error budget |
| SLA | Promise | Customers/Legal | Financial or legal penalties |
