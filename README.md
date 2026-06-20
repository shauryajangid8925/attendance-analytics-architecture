# 🎓 College Attendance Analytics & Governance System

A high-integrity, data-engineered attendance management and analytical pipeline built on a robust relational database structure, featuring fine-grained database-level access controls and real-time computation triggers.

---

## 🌐 Live System Architecture Access
* **Production Endpoint:** https://attend-hub.vercel.app/login
* *Note: To ensure data infrastructure integrity, public registration pipelines are disabled. Architectural auditing access (Student, Faculty, or Administrative credentials) is restricted. Please request transient credentials via a LinkedIn DM.*

---

## 🏗️ System Architecture & Data Engineering Stack

<img width="1536" height="1024" alt="attendance-analytics-architecture" src="https://github.com/user-attachments/assets/78731b89-b31a-4784-aeaf-fa867f7ad882" />


| Infrastructure Layer | Component / Technology |
| :--- | :--- |
| **Data Storage & Engine** | PostgreSQL (Hosted via Supabase Managed Engine) |
| **Security Execution** | Native Row Level Security (RLS) + Custom RPC Functions |
| **Application Layer** | React (Vite Framework) + Tailwind Architecture |
| **Deployment Pipelines** | Vercel Serverless Edge |

---

## 🧠 Relational Schema Design & Data Constraints

The system architecture relies on a 6-table normalized PostgreSQL relational schema designed to eliminate transactional operational anomalies. 

### 1. Schema Infrastructure
* `students`: Core dimensional table containing system-calculated student metrics.
* `teachers`: Faculty entity registry.
* `subjects`: Metadata schema separating analytical constraints (Theory vs Laboratory weights).
* `teacher_subjects`: Relational bridge (Junction Table) handling Many-to-Many entity mapping.
* `attendance`: Transactional log table handling daily event strings.
* `lab_group_mapping`: Dynamic lookup mapping database constraints across asymmetric branch structures.

### 2. Analytical Optimization & Edge-Case Modeling
The schema directly models complex real-world educational structures at the relational tier, eliminating application-layer computation overhead:
* **Conditional Structural Logic:** Handles branching paradigms natively. Computer Science (CSE) processes sub-group partitions (G1/G2) using composite junction filters, while Artificial Intelligence (AIML) executes queries over a flat, undivided relation.
* **Dynamic Weighting Computations:** Differentiates processing based on subject classification variables:
    * **Theory Events:** Evaluated with a transactional unit configuration where lectures count as 1.
    * **Laboratory Events:** Evaluated as a multi-period payload string where lectures count as 2.

---

## 📊 Automated Backend Aggregations (Calculations Layer)

Instead of pulling heavy, unindexed transaction logs to the client-side UI—which creates massive network bottlenecks—all data aggregation occurs directly inside the PostgreSQL query engine.

### Real-Time Metric Aggregation
A database trigger executes asynchronously on every data modification (INSERT or UPDATE operation within the attendance table). The computation computes raw analytical metrics across the historical timeline using the following logic rule:

$$\text{Attendance \%} = \left( \frac{\sum \text{Attended Lectures}}{\sum \text{Total Conducted Lectures}} \right) \times 100$$

* **Performance Metric Tuning:** The calculated aggregation output is automatically cached directly inside the student attendance percentage column. This structural materialization guarantees O(1) query retrieval times for high-volume student lookup operations.

---

## 🔐 Data Governance & Security Matrix

Security and data boundaries are structurally locked into the database layer via PostgreSQL **Row Level Security (RLS)**. Even if an actor manipulates network traffic or injects scripts into the frontend developer console, the query pipeline will reject unauthorized access at the engine layer.

### Row Level Security (RLS) & Access Management (RBAC)

| Database Relation | Principal (Super Admin) | Professor (Data Writer) | Student (Data Consumer) |
| :--- | :--- | :--- | :--- |
| `students` | Full CRUD Permissions | Read Access (Assigned Streams Only) | Read Access (Self-Bounded Payload Only) |
| `teachers` | Full CRUD Permissions | Read Access (Self Profile Isolation) | Access Strictly Denied |
| `subjects` | Full CRUD Permissions | Read Access (Assigned Relations Only)| Read Access (Self-Academic Path) |
| `attendance` | Full CRUD Permissions | Read / Write Access (Target Date = Current Date) | Read Access (Self Historical Record Only) |

### Enterprise Data Governance Rules
* **Bypassing Mitigations:** Open user-registration is disabled. Administrative boundaries are guarded by an isolated PostgreSQL Remote Procedure Call (RPC) function (`create_user_with_role`), executing systemic cross-table user generation with absolute transaction isolation.
* **Temporal Logic Locks:** Database constraints block data injection anomalies. Faculty accounts are locked via date validation queries restricting state changes exclusively to Current Date. Past or future entry mutations are blocked at the engine layer.

---

## ⚡ Data Pipeline Performance Optimizations

* **Server-Side Payload Pagination:** Constrains transactional data fetches to a strict maximum limit of 20 entity blocks per payload, keeping database pool memory cycles lean.
* **Network Query Debouncing:** Implemented a 300ms query deceleration barrier on all search operations to eliminate compute spikes and connection pool starvation within the Supabase Postgres instance.
* **Cascading Purge Chains:** Hard-deletion pipelines run under explicit cascading constraints, utilizing automated backend functions (`delete_auth_user()`) to perform complete database cleanups across independent schemas during node removal.

---

## 🔮 Future Analytics Roadmap
* [ ] **Predictive Trend Pipeline:** Designing an ML predictive model utilizing sequential historical regression to flag student academic drop-out risks based on localized multi-week attendance degradation curves.
* [ ] **Automated Reporting Architecture:** Building a Cron-scheduled background worker to auto-aggregate system states and deliver analytical reporting arrays directly to administration nodes.
