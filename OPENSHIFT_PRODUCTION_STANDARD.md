# OpenShift Bare-Metal Production Standard

**Document ID:** OCP-BM-STD-001  
**Version:** 0.2  
**Status:** Draft for architecture and control-owner review  
**Owner:** OpenShift Platform Service Owner  
**Applies to:** Production Red Hat OpenShift Container Platform (OCP) services deployed on bare-metal infrastructure  
**Review cadence:** Annually; and on material changes to OCP release, service scope, architecture, risk, or applicable standards

## 1. Purpose and conformance statement

This standard defines a reusable production architecture and implementation profile for an OpenShift-based container platform on bare metal. It combines:

1. A cloud reference-architecture view aligned to ISO/IEC 22123-3:2023.
2. An architecture description structure aligned to ISO/IEC/IEEE 42010:2022.
3. A set of implementation and evidence controls mapped to OpenShift capabilities and the surrounding service organization.

This document is a **standards-aligned implementation profile**, not an ISO certification, conformity assessment, or statement that an OpenShift installation alone implements every requirement of the referenced standards. ISO/IEC 22123-3 describes a cloud computing reference architecture; ISO/IEC/IEEE 42010 specifies requirements for architecture descriptions. Neither makes an individual cluster compliant by virtue of using OpenShift. Conformity requires an agreed scope, the complete applicable normative texts, documented architecture, implemented controls, objective evidence, and assessment by the responsible organization.

ISO/IEC 17789:2014 is listed for historical traceability only. ISO marks it withdrawn and identifies ISO/IEC 22123-3:2023 as its successor. This standard therefore uses ISO/IEC 22123-3:2023 as the current cloud reference-architecture basis.

OpenShift supplies platform mechanisms. The service provider remains responsible for people, process, facilities, contracts, risk acceptance, service levels, external dependencies, and evidence. Controls marked **External** cannot be completed by configuring OpenShift alone.

## Implementation template

Use [ocp_baremetal_architecture_and_deployment.md](./ocp_baremetal_architecture_and_deployment.md) to plan an Assisted Installer deployment, record its architecture, capture evidence, and complete post-installation acceptance. Copy the template per environment and replace all placeholders before approval.

## 2. Normative language, applicability, and control status

**MUST**, **SHOULD**, and **MAY** are normative terms for this project standard:

- **MUST**: required for production acceptance.
- **SHOULD**: expected unless a documented, approved exception exists.
- **MAY**: optional.
- **N/A**: permitted only with rationale, scope impact, approver, and review date.

Each control owner records one implementation status:

| Status | Meaning |
|---|---|
| Not assessed | No assessment has been completed. |
| Planned | An owner and target date exist; implementation is incomplete. |
| Implemented | The control is in place, but required evidence or effectiveness validation is incomplete. |
| Verified | Implementation and evidence have been reviewed, and required tests passed. |
| Exception | An authorized risk owner accepted a documented, time-bounded exception. |
| Not applicable | The control is outside the approved scope, with a rationale recorded. |

A production release MUST have every applicable MUST control marked **Verified** or **Exception**. Exceptions MUST identify the risk, impact, compensating controls, approver, expiry/review date, and tracking record. An exception is not evidence of conformity.

## 3. Standards register and interpretation

| Reference | Status and role in this document | Application |
|---|---|---|
| ISO/IEC 17789:2014, Information technology — Cloud computing — Reference architecture | Withdrawn; successor is ISO/IEC 22123-3:2023. Retained as a legacy cross-reference only. | Do not use as the current basis for new design decisions. |
| ISO/IEC 22123-1:2023, Information technology — Cloud computing — Part 1: Vocabulary | Current cloud vocabulary reference. | Use its defined terms when naming cloud roles and concepts in the architecture description. |
| ISO/IEC 22123-2:2023, Information technology — Cloud computing — Part 2: Concepts | Current cloud concepts reference. | Use to define the service and cloud concepts adopted by this service; document any tailoring. |
| ISO/IEC 22123-3:2023, Information technology — Cloud computing — Part 3: Reference architecture | Primary cloud reference architecture for this profile. | Describe applicable cloud roles, activities, functional components, and relationships; map them to OCP and external responsibilities. |
| ISO/IEC/IEEE 42010:2022, Software, systems and enterprise — Architecture description | Primary architecture-description reference. | Structure the architecture description around the system of interest, stakeholders, concerns, viewpoints, views, model kinds, correspondence, rationale, and decisions. |
| ISO/IEC 27001:2022 and applicable amendments | Information security management system (ISMS) requirements; organizational management-system standard. | Map applicable platform risks and controls into the organization’s ISMS and Statement of Applicability. OCP is one technical scope component, not an ISMS by itself. |
| ISO/IEC 27002:2022 | Information security control guidance. | Use as a control-design reference through the organization’s ISMS. |
| ISO/IEC 27017:2026 | Cloud-service information security control guidance based on ISO/IEC 27002. | Use where the organization is assessing cloud-service provider/customer responsibilities; validate applicability and current publication status at assessment time. |
| ISO/IEC 27018:2025 | PII protection guidance for public-cloud providers acting as PII processors. | Apply only where the service and data-processing roles are in scope; a private bare-metal cluster is not automatically a public cloud service. |
| ISO 22301:2019, including applicable amendments | Business continuity management system (BCMS) requirements. | Use for business impact, continuity strategy, exercise, and recovery governance beyond technical cluster recovery. |
| ISO/IEC 20000-1:2018, including applicable amendments | Service management system (SMS) requirements. | Use for service portfolio, service levels, incident/change/configuration and continual-improvement processes where included in scope. |

The organization MUST confirm the edition, amendments, applicability, and licensing of each standard before a formal audit. Standards are copyrighted; this document does not reproduce their normative text or make unsupported clause-by-clause compliance claims. Where a formal clause-level assessment is required, use the organization’s licensed copy and add clause identifiers to the control register.

## 4. System of interest and architecture context

### 4.1 System of interest

The system of interest is the **OpenShift Bare-Metal Container Platform Service**: the technical platform, its physical and network dependencies, platform operations, customer-facing interfaces, and governance needed to deliver production container workloads.

The architecture description MUST state whether the offering is operated as an internal private-cloud service, a managed service, or another service model. Bare-metal infrastructure or Kubernetes usage alone does not establish that a service meets a cloud definition or deployment-model criteria.

### 4.2 Scope boundary

The architecture description MUST identify what is inside and outside the service boundary. At minimum, assess:

- OCP control plane, compute nodes, Operators, APIs, console, and cluster configuration.
- Physical servers, firmware, BMC, power, racks, and facilities.
- Network fabrics, DNS, NTP, API and ingress load balancers, firewall, proxy, and external IPAM.
- Storage systems, CSI drivers, backup repositories, and recovery dependencies.
- Identity provider, privileged access, certificate authorities, secrets management, and image registries.
- Git repositories, CI/CD or GitOps systems, observability backends, ticketing, paging, and support channels.
- Application teams, workload services, data owners, and any external providers.

### 4.3 Cloud roles and shared responsibility

Use the terminology from ISO/IEC 22123-1 and the role relationships described by ISO/IEC 22123-3 in the approved architecture description. The following table is the project’s initial role allocation; replace role names and owners with the organization’s approved terms.

| Cloud role / participant | Expected participant in this service | Primary responsibility | Required interface or evidence |
|---|---|---|---|
| Cloud service customer | Application/product team or consuming business unit | Selects service options, classifies data, meets workload and security requirements, operates application behavior, owns service RTO/RPO. | Service request, workload onboarding, service catalog/SLA, application recovery evidence. |
| Cloud service provider | Organization operating and delivering the OpenShift service | Defines service, operates platform, capacity, security baseline, availability, support, lifecycle, and customer interface. | Service description, architecture description, operational controls, service reports. |
| Cloud service partner | Red Hat, server/storage/network vendors, identity provider, facility operator, backup provider, or contracted managed-service provider as applicable | Supplies an infrastructure, software, support, identity, or operational dependency under an agreed responsibility boundary. | Contract/support scope, compatibility, service dependency, escalation and shared-responsibility matrix. |
| Cloud service carrier | Network provider or internal network service carrying service traffic | Provides connectivity, routing, segmentation, and network availability between service actors and dependencies. | Network service definition, firewall/flow ownership, capacity and failure evidence. |
| Cloud service auditor | Internal audit, security assurance, or independent assessor where appointed | Assesses defined controls and evidence against the selected criteria. | Assessment scope, evidence requests, findings, remediation tracking. |
| Cloud service broker | Include only if an organization or service actually brokers, aggregates, or mediates services. | Any brokerage activity and its responsibility boundary. | Broker role and service-chain diagram, or documented N/A rationale. |

No participant may be assumed to own a control simply because it supplies a product. The service provider MUST maintain a control-responsibility matrix and confirm shared responsibilities with customers and partners.

### 4.4 Service model and deployment model

The architecture description MUST document:

- The customer-visible service model (for example, a managed container platform / CaaS offering, with any PaaS-like capabilities explicitly listed).
- The deployment model and the criteria used to classify it; do not infer “private cloud” from on-premises location alone.
- The service catalog, service tiers, quotas, support hours, service-level objectives, and customer responsibilities.
- Which OCP features are offered, restricted, optional, or unsupported by the service.
- Data classification, jurisdiction, tenancy, and service-chain dependencies.

## 5. Architecture description structure aligned to ISO/IEC/IEEE 42010

The architecture description (AD) for each production environment MUST include the following sections. A diagram without stakeholders, concerns, explanatory text, rationale, and evidence is not a complete architecture description.

| AD element | Required content |
|---|---|
| Identification and scope | AD title, version, status, author/owner, approval date, system of interest, intended use, environment, OCP version, and exclusions. |
| Stakeholders | Customers, platform engineering, operations, security, enterprise architecture, application owners, auditors, vendors, network, storage, identity, and business continuity representatives. |
| Concerns | Availability, performance, security, data protection, sovereignty, tenancy/isolation, capacity, recoverability, maintainability, lifecycle, interoperability, supportability, cost, and compliance obligations. |
| Context view | System boundary; users, external services, providers, trust boundaries, entry points, and key information/data flows. |
| Functional/component view | Cloud-service activities and functional components; OCP control-plane, worker, Operators, APIs, automation, and service-management components. |
| Deployment/infrastructure view | Physical topology, nodes, racks, failure domains, BMC, networks, load balancers, DNS/NTP, storage, backup targets, and site dependencies. |
| Security/trust view | Identity flows, privileged paths, trust boundaries, tenant isolation, secrets, certificates, registry/image trust, egress, audit, and threat assumptions. |
| Data/storage view | Data classification, storage classes, persistence, replication, encryption, backup/restore, retention, ownership, and data lifecycle. |
| Operations/service-management view | Service catalog, provisioning, monitoring, logging, incident/change, patching, support model, maintenance, and customer interfaces. |
| Resilience/recovery view | Failure domains, availability design, degraded modes, dependencies, RTO/RPO, recovery sequence, and test scenarios. |
| Lifecycle/change view | Installation method, GitOps/configuration sources, Operator lifecycle, update channel, change authority, drift handling, and retirement. |
| Model kinds and notation | Legend, diagram/model types, notation, viewpoints, tool/source, and conventions used. |
| Correspondence and consistency | Cross-view relationships, identifier references, constraints, unresolved inconsistencies, and traceability to requirements and risks. |
| Rationale and decisions | Decision records with context, alternatives, decision, consequences, owner, date, and affected views/controls. |
| Assumptions and limitations | Capacity assumptions, design limits, exclusions, technology constraints, and items requiring validation. |
| Conformance/evaluation record | Assessment scope, criteria/standard editions, method, evidence, findings, exceptions, reviewer, and conclusion. |

The AD owner MUST keep model identifiers stable enough to trace a concern to one or more views, decisions, implementation controls, evidence items, and risks.

## 6. ISO/IEC 22123-3 cloud reference architecture mapping

This is a service-level mapping to the concepts of the cloud computing reference architecture. It is not a substitute for the standard’s normative content.

| CCRA aspect | OpenShift service realization | What OCP provides | Provider/customer/partner action and evidence |
|---|---|---|---|
| Cloud service roles and relationships | Service responsibility and dependency model from Section 4.3. | Technical APIs and control points do not assign organizational accountability. | Approve named role owners, contracts, service boundaries, support contacts, and shared-responsibility matrix. |
| Cloud service customer activities | Project/namespace request, workload onboarding, self-service deployment, quota consumption, monitoring, and support request. | Project APIs, Kubernetes APIs, quota/limit controls, RBAC, console, and optional service catalog/GitOps. | Publish service catalog, access workflow, quotas, customer obligations, support process, and workload acceptance criteria. |
| Cloud service provider activities | Service design, provisioning, operation, monitoring, security, capacity, change, incident, continuity, and retirement. | Operators, APIs, monitoring stack, Machine Config Operator, RBAC, network/security/storage primitives, and update mechanisms. | Define service processes, ownership, staffing, procedures, service metrics, and reviewed evidence. |
| Cloud service partner activities | Physical hosting, hardware, storage, network, identity, backup, software support, and managed operations. | Integration points and supported interfaces; OCP cannot enforce partner contracts or facility controls. | Validate supportability, service-level dependencies, escalation, changes, access, assurance, and recovery responsibilities. |
| Service orchestration and provisioning | Declarative install/configuration, cluster Operators, project creation, GitOps, and application delivery. | Installer, APIs, Operators, templates, and supported GitOps integration. | Version-controlled desired state, approval gates, provisioning process, secrets handling, and drift/emergency-change procedure. |
| Resource abstraction and control | API objects for compute scheduling, storage claims, services, routes, and policies. | Kubernetes/OpenShift API, scheduler, CSI, CNI, ingress, quotas, and machine configuration. | Define supported service options, quotas, capacity limits, supported storage/network classes, and exception process. |
| Physical resource layer | Bare-metal servers, firmware, power, racks, network, and storage systems. | Node management and integration abstractions; hardware remains externally operated. | Maintain inventory, compatibility, facility assurance, capacity, failure-domain design, vendor support, and replacement procedures. |
| Cloud service management | Business support, service catalog, provisioning/configuration, service levels, incidents, changes, monitoring, metering/capacity, and reporting. | Platform telemetry, APIs, events, and integration hooks. | Integrate ticketing, CMDB/inventory, paging, finance/capacity reporting, SLA/SLO, service ownership, and continual improvement. |
| Security and privacy | Identity, authorization, isolation, secure configuration, data protection, logging, and incident response. | OAuth/RBAC, SCC, admission, NetworkPolicy, encryption and certificate capabilities, audit events, and security Operators/features. | Set policy, operate identity and key management, classify data, assess risk, retain evidence, and meet privacy/legal obligations. |
| Interoperability and portability | Standard APIs, declarative resources, container image and storage/network interfaces, and documented dependencies. | Kubernetes/OpenShift APIs and supported integration interfaces. | Test application portability, document vendor-specific APIs/Operators/storage, keep deployment sources, and define export/exit strategy. |

## 7. Implementation control register

The controls below form the project’s minimum implementation baseline. Each row states the implementation outcome, the OpenShift mechanism (where applicable), controls outside OpenShift, and evidence required. The environment-specific register MUST add a named owner, implementation status, evidence location, last review, and next review to each row.

| ID | Requirement and acceptance outcome | OpenShift mechanism / implementation pattern | External responsibility and minimum evidence |
|---|---|---|---|
| ARC-01 | MUST maintain an approved architecture description with the views and traceability in Section 5. | OCP architecture docs, cluster manifests, GitOps repository, architecture diagrams. | Enterprise/platform architect approves AD; versioned diagrams, decision records, review history. |
| ARC-02 | MUST identify customer, provider, partner, carrier, auditor, and broker roles as applicable and assign controls. | Namespaces/projects, RBAC, service APIs, monitoring and tenant boundaries support role separation. | Signed responsibility matrix, contracts, service catalog and support/escalation ownership. |
| SVC-01 | MUST define the service model, deployment model, catalog, service tier, support hours, customer obligations, and SLO/SLA. | Projects, quotas, limit ranges, NetworkPolicy, storage classes and service templates can implement catalog options. | Service owner publishes service description, service-level definitions, request and fulfillment workflow. |
| LCM-01 | MUST run a supported OCP release and supported configuration; MUST track OCP and Operator lifecycles. | ClusterVersion channel, ClusterOperators, OperatorHub/catalog source controls. | Lifecycle review, subscription, compatibility/third-party validation, upgrade calendar and approval. |
| INF-01 | MUST use supportable hardware and record node, firmware, boot, disk, NIC, HBA/RAID, BMC, rack, power, and support data. | Node labels/roles, MachineConfig, cluster inventory APIs where applicable. | Hardware compatibility check, CMDB, vendor support, facility and BMC controls. |
| INF-02 | MUST use a documented, release-supported bare-metal installation method and reproducible provisioning process. | Installer/agent/user-provisioned deployment path as supported by selected release; declarative configuration. | Versioned install artifacts, secrets handled outside Git, DNS/IPAM/provisioning evidence, build and node-replacement runbook. |
| HA-01 | Standard HA profile MUST have three control-plane nodes; MUST document quorum and failure-domain design. Exceptions for compact/single-node are risk-approved and identify reduced availability. | Three-member control plane and etcd operated by OCP; health surfaced through Operators and alerts. | Topology diagram, capacity/failure analysis, maintenance/drain plan and approved exception if profile differs. |
| HA-02 | MUST retain enough worker and platform capacity after the largest planned failure to serve the approved critical workload set. | Scheduler, requests/quotas, topology spread, affinity/anti-affinity, autoscaling if supported. | Load/capacity model, utilization and headroom thresholds, workload tests, failure test results. |
| NET-01 | MUST approve non-overlapping node, machine, pod, service, storage, management, and external address ranges. | Cluster network configuration, OVN-Kubernetes, Multus/additional networks where required. | IPAM records, route/VLAN/VRF design, address-overlap check and reviewed network diagram. |
| NET-02 | MUST make required DNS, NTP, API and application ingress available and resilient for the chosen installation topology. | Cluster DNS and Ingress Operators, Routes, API/ingress endpoints. | DNS/NTP/load-balancer configuration, health checks, redundancy/failover evidence and owners. |
| NET-03 | MUST document firewall flows, egress, proxy and MTU; allow only approved connectivity. MUST define namespace network-policy baseline. | NetworkPolicy, EgressFirewall/EgressIP or supported network controls, cluster proxy configuration. | Reviewed flow matrix, firewall/proxy change records, MTU/connectivity tests, policy exceptions. |
| STO-01 | MUST use OCP-supported storage and document performance, failure behavior, encryption, data placement, capacity, and vendor support. | CSI drivers, StorageClasses, PVCs, snapshots only where supported; local storage only with node-loss plan. | Compatibility/support proof, storage service design, performance/capacity tests, failure and recovery analysis. |
| STO-02 | MUST define backup and tested restore for stateful application data; snapshots alone do not satisfy independent backup. | CSI snapshot integration where supported; OADP/approved backup integration where applicable. | App-consistent backup policy, separate failure-domain copy, retention, access control, successful restore report. |
| IAM-01 | MUST integrate approved identity, least-privilege RBAC, controlled break-glass access, periodic access review, and credential lifecycle. | OAuth identity providers, groups, RBAC roles/bindings, dedicated service accounts. | Identity-provider owners, privileged-access approvals, access review records, break-glass test and credential rotation. |
| SEC-01 | MUST keep workloads in approved projects, use least-privilege SCC/admission settings, and control privilege exceptions. | SCC, Pod Security Admission, RBAC, ResourceQuota, LimitRange, admission policy where supported. | Baseline policy, exception records, security review and recurring namespace/workload checks. |
| SEC-02 | MUST protect secrets, certificates, credentials, and API tokens; prevent secret leakage in Git, logs, or images. | Kubernetes Secrets, encryption at rest configuration, service-account controls, cert management integrations. | KMS/key ownership, approved external secret manager, rotation/revocation, access logs and leak-response procedure. |
| SEC-03 | MUST define trusted image sources, scanning, provenance, promotion, patching, and production image immutability. | Image registry integration, Image/Project policies, image pull secrets, admission controls where supported. | Registry controls, scan/signature evidence, SBOM/provenance, exception/remediation timelines. |
| SEC-04 | MUST preserve audit-relevant events and route required logs to durable external storage under an approved retention and access policy. | OCP audit, supported cluster logging and monitoring integrations. | External log destination, retention/immutability, access review, alert and incident investigations. |
| SEC-05 | MUST review vulnerability/advisory notices and remediate OCP, Operators, nodes, and workloads within organizational timeframes. | OCP update service, ClusterVersion, Operators, node configuration lifecycle. | Defined severity SLAs, exception/compensating-control record, patch reporting and escalation. |
| WRK-01 | MUST enforce application resource governance and safe scheduling for the workload criticality. | Requests/limits, quotas, topology spread, affinity/anti-affinity, probes, PDBs, rollout strategies, HPA where appropriate. | Workload standards, capacity tests, reviewable manifests, application service owner and service objective. |
| OBS-01 | MUST monitor platform and production workloads, route actionable alerts to a staffed owner, and test alert delivery. | Cluster monitoring, optional user-workload monitoring, Prometheus/Alertmanager APIs. | On-call rotation, alert ownership/severity, external paging integration, notification test, SLO reporting. |
| OBS-02 | MUST collect operational and application logs needed for support and security investigations. | Supported OCP logging integration and cluster events/metrics. | External log platform, retention, data access/classification, troubleshooting runbooks. |
| BCP-01 | MUST define approved RTO/RPO and recovery plans for cluster service and each critical workload. | etcd backup procedure for cluster state; declarative rebuild/reconciliation; platform monitoring. | Business impact analysis, application data backup, independent backup location, recovery owners and exercise report. |
| BCP-02 | MUST test restoration and record achieved recovery point, elapsed time, failures, and corrective actions. | Release-specific etcd recovery process and supported application recovery integration. | Annual minimum restore exercise and after material backup changes; signed test results and remediation tracking. |
| CHG-01 | MUST use a reviewed change process for upgrades, Operators, network/storage/security changes, and node configuration. | OCP update channels, Operators, MachineConfig Operator, GitOps, cluster APIs. | Change authority, impact analysis, maintenance windows, approvals, customer communication, test and completion records. |
| CHG-02 | MUST have a supported update plan; cluster downgrade MUST NOT be treated as rollback. | Update graph/channel and ClusterVersion conditions; release-specific update procedure. | Pre-update health/capacity/backup review, staging where available, upgrade runbook, support case criteria. |
| OPS-01 | MUST operate incidents, problems, requests, configuration/inventory, capacity, and continual improvement as a service. | OCP events, alerts, APIs, audit trail and automation hooks. | Service-management process, ticket/CMDB/paging integrations, owners, reports, support entitlement and runbooks. |
| PORT-01 | MUST document portability limits and exit/rebuild approach for workloads, data, operators, network and storage dependencies. | Declarative Kubernetes/OpenShift APIs, GitOps, OCI images and standard CSI/CNI interfaces where applicable. | Tested export/rebuild path, dependency inventory, vendor lock-in review, data export/retention/deletion process. |
| MESH-01 | MUST assess service-mesh need and use only an OCP/RHOSSM combination supported for the target release and production use. Mesh is optional, not a default. | RHOSSM/Istio for workload identity, traffic policy, mTLS and telemetry. | Approved use case, compatibility/entitlement, owner and architecture decision. |
| MESH-02 | MUST opt workloads into sidecar injection by approved namespace or workload labels and associate them with an intentional IstioRevision. Exclude platform namespaces unless supported and approved. | RHOSSM Operator, Istio resources, revision labels and injection controls. | Version-controlled membership/label inventory and verification of injected pods and control-plane association. |
| MESH-03 | MUST define workload identity, mTLS mode, authorization, certificates/trust, gateways and NetworkPolicy interaction. Stage strict enforcement. | Istio security policies/gateways and OCP NetworkPolicy. | Threat model, certificate owner, policy tests, exception record; mesh does not replace perimeter controls. |
| MESH-04 | MUST budget proxy resources and validate probes, startup/shutdown, connection drain, PDBs, topology and application performance. | Sidecars are injected into application pods and consume pod/node resources. | Capacity and load tests, resource budgets, rollout/PDB review, application-owner sign-off. |
| MESH-05 | MUST monitor mesh/proxy health, traffic, certificate expiry, policy denials and resource use; control telemetry/log access and retention. | Mesh metrics/log integrations with supported OCP observability. | Dashboards, alert owner, retention/access policy and incident runbooks. |
| MESH-06 | MUST stage control-plane and sidecar upgrades with a supported RHOSSM strategy and tested recovery path. | RHOSSM Operator update strategy and revision management. | Compatibility, staged upgrade, revision migration, rollback and decommission evidence. |
| OPR-01 | MUST inventory purpose, owner, catalog/source, version/channel, scope, dependencies, support status and lifecycle for every Operator. | CVO manages core ClusterOperators; OLM/OperatorHub manages add-ons. | Approved catalog, entitlement/support review, change record and inventory export. |
| OPR-02 | MUST distinguish core, Red Hat, certified vendor and community Operators; do not independently install/update CVO-managed core Operators. | ClusterOperator/CVO and OLM lifecycle mechanisms. | Support boundary, source trust, owner and cluster inventory review. |
| OPR-03 | MUST install only use-case-justified Operators supported for exact OCP release/platform and tested for security, resource, update and uninstall impact. | OperatorHub/OLM or an explicitly supported Assisted Installer option. | Compatibility matrix, stage test, privilege/dependency review, maintenance and recovery plan. |
| OPR-04 | MUST assign an operational owner/runbook for Operator custom resources, CRDs, credentials, upgrades and decommissioning. | Reviewed GitOps/configuration for custom resources and subscriptions. | Access review, backup/export, support contact, alerting and update/rollback runbook. |
| RSK-01 | MUST maintain a risk and exception register tied to concerns, architecture decisions, controls, owners, and review dates. | Cluster evidence and configuration can support technical risk monitoring. | Risk owner, risk acceptance, assurance/audit process, remediation tracking and expiry enforcement. |

## 8. Service Mesh / Sidecar Management

Service Mesh is optional. Use it when a workload needs service-to-service identity, mutual TLS, traffic policy, or mesh telemetry. It adds a proxy and control-plane lifecycle to application operations, so injection MUST NOT be enabled cluster-wide by default.

### 8.1 Design and membership

- Record the service-mesh use case, product/version, OCP compatibility, entitlement, support status, control-plane namespace, member namespaces, gateways, external dependencies, and owner.
- Select sidecar mode or another generally available and supported production mode. Do not use Developer Preview or Technology Preview features for production workloads.
- Sidecar injection MUST be opt-in, scoped to approved application namespaces or pod templates, and associated with the intended IstioRevision. Do not assume the revision is named default.
- Exclude platform/system namespaces unless the product documentation supports that use and the platform owner approves it.
- Keep namespace/workload membership labels and Istio custom resources in reviewed version control.
- A namespace label affects newly created pods; use an approved rollout to recreate pods and verify the actual pod contains the expected proxy.

Illustrative checks; validate commands and label syntax against the installed RHOSSM release:

~~~bash
oc get istio -A
oc get istiorevisions
oc describe pod <pod-name> -n <application-namespace>
~~~

For a non-default control-plane revision, RHOSSM commonly uses a revision label in the form istio.io/rev=<revision-name>. Confirm the exact revision and label precedence before applying changes.

### 8.2 Security, reliability, lifecycle, and evidence

- MUST define workload identity, mTLS mode, authorization, certificate issuer/trust/rotation, gateway exposure, and egress.
- MUST stage the transition to strict mTLS; test all dependencies before enforcement.
- MUST document interaction with NetworkPolicy, ingress, external load balancers, firewall, API gateway/WAF, and application authorization. Mesh does not replace these controls.
- MUST budget CPU, memory, ephemeral storage and network overhead for proxies and control plane; include this in capacity, autoscaling, probes and performance testing.
- MUST validate readiness/startup/shutdown behavior, connection draining, PDBs, topology spread, rolling upgrades, and workload availability with proxies injected.
- MUST monitor control-plane and proxy health, traffic errors/latency, certificate expiry, policy denials and resource saturation. Protect telemetry and log retention.
- MUST stage control-plane and sidecar updates with the supported RHOSSM strategy. Maintain onboarding, upgrade, rollback, incident, and decommission runbooks.

| Design item | Selected value / evidence |
|---|---|
| Product/version, OCP compatibility and entitlement | {{REFERENCE}} |
| Use case and workload namespaces | {{REFERENCE}} |
| Control plane / IstioRevision / update strategy | {{VALUE}} |
| Injection labels, source repository and exclusions | {{REFERENCE}} |
| Identity, mTLS, authorization, certificates | {{REFERENCE}} |
| Gateways, NetworkPolicy and egress | {{REFERENCE}} |
| Proxy resources, availability/performance test | {{REFERENCE}} |
| Monitoring, alerts, telemetry access/retention | {{REFERENCE}} |
| Upgrade, rollback and removal runbooks | {{REFERENCE}} |

## 9. Common OpenShift Operators

This is a planning catalog, not a mandatory installation list. An Operator MUST have a justified use case, support/compatibility check for the exact OCP release/platform, an owner, resource/security review, and an upgrade/removal plan.

### 9.1 Operator classes

1. **Core ClusterOperators:** OCP functions managed by the Cluster Version Operator (CVO). Inventory them; do not install duplicate versions from OperatorHub or independently manage their lifecycle.
2. **Red Hat add-on Operators:** use the product’s documented installation path and confirm entitlement and support lifecycle.
3. **Certified vendor Operators:** validate certification, vendor support scope, data access, privileges, dependencies, and compatibility.
4. **Community Operators:** treat as community-maintained unless the current listing explicitly establishes otherwise; require additional risk and lifecycle review before production.
5. **Assisted Installer selections:** availability and support status depend on release/platform. Do not use an option marked Developer Preview or Technology Preview for production. If advanced configuration is needed, install after cluster creation only using a supported product procedure.

### 9.2 Common optional Operators by use case

| Operator / capability | Typical use | Bare-metal design consideration |
|---|---|---|
| OpenShift GitOps | Declarative app/platform delivery and drift reconciliation | Protect repository credentials; define bootstrap, ownership and recovery. |
| OpenShift Pipelines | Tekton CI/CD | Isolate builders; govern registry secrets, provenance, artifacts and resource quotas. |
| OpenShift Logging | Workload/platform log collection and forwarding | Select a supported backend, external durability, retention and access controls. |
| OpenShift Service Mesh | Service identity, mTLS, traffic management and telemetry | Apply Section 8; verify supported version and installation path. |
| OpenShift Data Foundation (ODF) | Software-defined block/file/object storage | Validate disks, failure domains, network, capacity, licensing and storage operations. |
| Local Storage Operator (LSO) | Local persistent volumes from node-attached devices | Local volumes are node-bound; document placement and node/disk-loss recovery. |
| Logical Volume Manager Storage (LVMS) | Local LVM-backed storage for supported use cases | Validate topology and limits; do not treat it as shared or replicated storage. |
| Kubernetes NMState | Declarative Day-2 node networking | Review interfaces, bridges, VLANs, routes, MTU and Machine Config interaction. |
| MetalLB | LoadBalancer service IPs on bare metal | Approve address pools and L2/BGP design. It does not automatically replace API/Ingress load balancing. |
| OpenShift API for Data Protection (OADP) | Cluster/application backup integrations | Test application consistency, object storage, credentials, retention and restore. |
| Compliance Operator | Scan against selected compliance profiles | Choose applicable profiles; triage and test remediations before enforcement. |
| cert-manager | Workload certificate issuance/renewal | Validate issuer trust and renewal alerts; distinguish it from OCP platform certificate management. |
| OpenShift Virtualization | Run VMs on OCP | Validate CPU, storage/network, migration, backup and support requirements. |
| Node Feature Discovery / SR-IOV Network Operator | Hardware discovery and high-performance networking | Use for explicit workload requirements; validate NIC/switch config, device resources and node lifecycle. |
| Red Hat Quay | Enterprise image registry | Plan persistent/object storage, database, TLS, identity, scanning, backup and mirroring. |

“Common” does not mean required, included in every SKU, or supported in every topology. Confirm the current OperatorHub/catalog listing, product documentation, entitlement, Red Hat Ecosystem Catalog, and support policy.

### 9.3 Operator inventory

For every installed Operator, maintain: product/name and publisher; source/catalog; use case/decision; supported OCP versions/architecture/platform; entitlement; channel/version/update approval; namespace/scope; CRDs/custom resources/dependencies; RBAC/secrets/webhooks/network/storage access; capacity impact; service owner/support contact; monitoring; backup/export; upgrade/rollback/decommission procedure; last and next compatibility review.

## 10. Minimum operational procedures

The service provider MUST maintain version-controlled, release-aligned procedures for:

1. Cluster installation and rebuild, including infrastructure and dependency prerequisites.
2. Node onboarding, replacement, decommissioning, and secure media handling.
3. Routine cluster updates, emergency security updates, Operator lifecycle, and failed-update response.
4. Node cordon/drain and maintenance, including PDB and storage behavior.
5. Identity-provider outage and break-glass access.
6. API, ingress, DNS, network, storage, and certificate incidents.
7. etcd backup and supported recovery for the exact OCP release.
8. Application data backup/restore, cluster rebuild, and service recovery.
9. Security incident, vulnerability remediation, and audit evidence preservation.
10. Diagnostic bundle collection, sensitive data handling, Red Hat support engagement, and vendor escalation.
11. Emergency configuration changes, GitOps drift reconciliation, and post-incident review.
12. Capacity threshold review, scaling, and service/customer communication.

## 11. Production acceptance and verification

### 11.1 Acceptance gates

A production environment MUST NOT be handed over until:

- The architecture description in Section 5 is approved and its views agree with the deployed topology.
- All in-scope MUST controls are Verified or have an approved, time-bounded Exception.
- OCP and Operators are supported, and lifecycle and update ownership is assigned.
- Control-plane topology, compute headroom, network, DNS, load balancing, storage, identity, and backup dependencies are evidenced.
- Cluster health is acceptable; no unresolved critical operator, node, storage, security, or capacity condition remains.
- Authentication, authorization, privileged access, security exceptions, audit collection, alert routing, and log retention are reviewed.
- Backup restoration and at least one failure/continuity exercise have evidence, and approved RTO/RPO are achievable.
- Runbooks, service catalog, service owners, support contacts, and customer responsibilities are published.
- Risks, assumptions, limitations, and decisions are approved and traceable.

### 11.2 Suggested OCP health snapshot

Run with an authorized account and attach output or a suitably redacted summary to the acceptance record. This is point-in-time evidence, not a full audit:

~~~bash
oc get clusterversion
oc get clusteroperators
oc get nodes -o wide
oc get machineconfigpools
oc get storageclass
oc get pods -A
oc get events -A --sort-by=.lastTimestamp
~~~

### 11.3 Assessment record

For every assessment, record:

| Field | Required value |
|---|---|
| Assessment scope | Cluster, service, environment, boundaries, exclusions |
| Criteria | Standard title, edition/amendments, selected controls and licensed clause references where applicable |
| Method | Interview, configuration review, observation, sampling, test, or external assurance |
| Control result | Status from Section 2, rationale, and affected architecture view |
| Evidence | Stable repository/ticket/report location, date, owner, and integrity/access classification |
| Finding | Gap, impact/risk, corrective action, owner, target date, retest result |
| Approval | Assessor, control owner, risk owner, approval date, next review |

## 12. Project architecture description template

Copy and complete this template for each environment. “TBD” is not a production acceptance value.

### 12.1 Identification

- Environment / cluster name:
- Service owner:
- AD owner, version, approval date:
- OCP version, channel, architecture:
- Installation method:
- Service model and deployment model:
- Scope and exclusions:

### 12.2 Stakeholders and concerns

| Stakeholder | Role | Concerns | Required view / evidence | Owner |
|---|---|---|---|---|
| Business/service customer | TBD | Availability, data, service level, support | Service catalog, SLO/SLA, recovery evidence | TBD |
| Platform provider | TBD | Supportability, capacity, security, lifecycle | Component, operations, and lifecycle views | TBD |
| Security / risk | TBD | Identity, isolation, threat, audit, privacy | Security/trust view, risk register | TBD |
| Infrastructure partners | TBD | Hardware, network, storage, facility, support | Deployment and dependency views | TBD |
| Application owners | TBD | Performance, deployment, data, recovery | Workload and data views | TBD |
| Auditor / assessor | TBD | Scope, criteria, evidence, findings | Assessment record | TBD |

### 12.3 Architecture viewpoints and views

| View ID | Viewpoint / concern set | Model or diagram | Source of truth | Approval |
|---|---|---|---|---|
| V-CTX | System context, roles, external parties, trust boundaries | Context/dependency diagram | TBD | TBD |
| V-FUNC | Cloud activities, functional components, APIs | Functional/component diagram | TBD | TBD |
| V-DEP | Nodes, racks, network, load balancers, storage, facilities | Physical/deployment diagram | TBD | TBD |
| V-SEC | Identity, authorization, isolation, threat assumptions, audit | Trust/data-flow diagram | TBD | TBD |
| V-DATA | Classification, persistence, data movement, backup/restore | Data lifecycle diagram and matrix | TBD | TBD |
| V-OPS | Provision, monitor, change, incident, support, service levels | Service operations model | TBD | TBD |
| V-RES | Failure domains, degraded modes, dependencies, RTO/RPO | Recovery/dependency diagram | TBD | TBD |
| V-LCM | GitOps, updates, Operators, drift, retirement | Lifecycle/change flow | TBD | TBD |

### 12.4 Architecture decisions and correspondence

For each architecture decision, record: ID; date; context/concern; decision; alternatives; consequences; evidence; approver; related controls; affected view IDs; review trigger.

For each major requirement or risk, record: ID; stakeholder concern; architecture view; decision; implementation control ID; evidence location; assessment result.

## 13. Supporting implementation standards and references

Supporting standards provide organization-level or domain-specific criteria. They do not become OpenShift configuration requirements unless the service scope and risk assessment make them applicable.

| Standard / source | Use in the implementation profile |
|---|---|
| ISO/IEC 27001 and ISO/IEC 27002 | ISMS risk treatment, control ownership, access, operations, supplier, incident, and evidence processes. OCP technical configuration supplies evidence for some controls but not the management system. |
| ISO/IEC 27017 | Cloud-specific allocation and implementation guidance for provider and customer responsibilities. Use the current edition applicable at the assessment date. |
| ISO/IEC 27018 | PII controls where the service is a public-cloud PII-processing service and the organizational data-processing scope requires it. |
| ISO 22301 | Business impact, continuity strategy, roles, exercise, and recovery governance; technical OCP/etcd and application restore procedures are supporting evidence. |
| ISO/IEC 20000-1 | Service management processes, service catalog/levels, incident/change/configuration, supplier management, and continual improvement. |

A security baseline such as a release-matched Center for Internet Security (CIS) OpenShift benchmark MAY be used as a technical hardening reference if the organization has selected it. Record the exact benchmark version, compatibility, exceptions, and validation method. Do not represent a CIS benchmark as an ISO standard or apply settings that conflict with Red Hat support guidance.

## 14. References

### 14.1 ISO and IEC sources

1. [ISO/IEC 17789:2014 — official status page; withdrawn and successor identified](https://www.iso.org/standard/60545.html)
2. [ISO/IEC 22123-1:2023 — Cloud computing vocabulary](https://www.iso.org/standard/82758.html)
3. [ISO/IEC 22123-2:2023 — Cloud computing concepts](https://www.iso.org/standard/80351.html)
4. [ISO/IEC 22123-3:2023 — Cloud computing reference architecture](https://www.iso.org/standard/82759.html)
5. [ISO/IEC/IEEE 42010:2022 — Architecture description](https://www.iso.org/standard/74393.html)
6. [ISO/IEC 27001:2022 — Information security management systems](https://www.iso.org/standard/27001)
7. [ISO/IEC 27002:2022 — Information security controls](https://www.iso.org/standard/75652.html)
8. [ISO/IEC 27017:2026 — Cloud-service security controls](https://www.iso.org/standard/27017)
9. [ISO/IEC 27018:2025 — PII protection in public clouds](https://www.iso.org/standard/27018)
10. [ISO 22301:2019 — Business continuity management systems](https://www.iso.org/standard/75106.html)
11. [ISO/IEC 20000-1:2018 — Service management system requirements](https://www.iso.org/standard/70636.html)

### 14.2 Red Hat implementation sources

Use the documentation matching the deployed OCP release. These OCP 4.22 sources are an initial reference set for this draft; confirm the current supported release and replace the version in links during environment onboarding.

1. [OpenShift Container Platform architecture](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/architecture/)
2. [Installing on bare metal](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/installing_on_bare_metal/)
3. [Release notes](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/release_notes/)
4. [OpenShift Container Platform life cycle policy](https://access.redhat.com/support/policy/updates/openshift)
5. [Updating clusters](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/updating_clusters/)
6. [Backing up and restoring etcd](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/etcd/backing-up-and-restoring-etcd-data)
7. [Backup and restore](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/backup_and_restore/)
8. [Authentication and authorization](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/authentication_and_authorization/)
9. [Security and compliance](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/security_and_compliance/)
10. [Storage](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/storage/)
11. [Networking overview](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/networking_overview/)
12. [Network security](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/network_security/)
13. [Machine configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/machine_configuration/)
14. [Monitoring stack for Red Hat OpenShift](https://docs.redhat.com/en/documentation/monitoring_stack_for_red_hat_openshift/4.22/)
15. [OpenShift Operator life cycles](https://access.redhat.com/support/policy/updates/openshift_operators)
