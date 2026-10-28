# OpenShift Production Quality Standard

**Status:** Initial draft  
**Version:** 0.1  
**Applies to:** Red Hat OpenShift Container Platform (OCP) clusters running production workloads on bare-metal infrastructure  
**Review cadence:** At least annually, and whenever the supported OCP release or the platform architecture changes

## 1. Purpose

This document defines the minimum platform and operational controls required before an OpenShift bare-metal cluster is accepted for production. It is a baseline for architecture, deployment, security, operations, and review. It is not a substitute for the installation guide, hardware compatibility list, product support policy, or application-specific requirements.

The cluster owner must record the selected values and evidence for every requirement below. If a requirement does not apply, record the reason and the approved compensating control.

## 2. Normative language and acceptance

- **MUST** means required for production acceptance.
- **SHOULD** means expected unless a documented exception is approved.
- **MAY** means optional.

A cluster is production-ready only when:

1. All MUST requirements have an owner, an implemented control, and reviewable evidence.
2. Required restore and failure procedures have been exercised and meet the agreed recovery objectives.
3. No unresolved critical or high risks remain without a named approver, mitigation, and due date.
4. The deployment uses a currently supported OCP release and only supported configurations and Operators.

The cluster design record must include the OCP version and channel, installation method, topology, network ranges, storage classes, identity provider, backup targets, recovery objectives, maintenance window, service owners, and support/escalation contacts.

## 3. Platform and lifecycle

| Area | Minimum standard | Evidence |
|---|---|---|
| Product support | MUST run a generally available OCP release within its published support lifecycle. MUST check the OCP and Operator lifecycle policies before deployment and on a regular schedule. Technology Preview features MUST NOT carry production workloads. | Version, channel, support entitlement, lifecycle review date, and list of Operators/features |
| Supported design | MUST use hardware, firmware, drivers, installation method, and integrations supported for the selected OCP release. MUST record vendor support contacts and any dependency on third-party support. | Compatibility validation and architecture record |
| High availability | Standard production profile MUST use three control-plane nodes. Compute capacity MUST tolerate the loss of the largest planned failure unit (at minimum one worker) while maintaining the agreed critical workload capacity. Single-node and compact profiles require an explicit availability exception and business approval. | Node / failure-domain diagram and capacity calculation |
| Failure domains | SHOULD spread control-plane and critical workload capacity across independent power, rack, and network failure domains where the data center design permits. | Rack, power, and network placement |
| Capacity | MUST size CPU, memory, ephemeral storage, pod density, and persistent storage from measured or forecast demand, including platform overhead, peak load, maintenance, and failure headroom. MUST define alert thresholds before reaching exhaustion. | Capacity model and alert thresholds |
| Time | MUST provide reliable time synchronization to all nodes and dependent infrastructure. | NTP/chrony source and synchronization check |
| Supportability | MUST have a valid subscription/support path, an inventory of hardware/software, and a tested process to collect diagnostics and open a support case. | Asset inventory and support runbook |

Do not prescribe a universal worker count or node size: the required number depends on workload demand, redundancy targets, hardware, and licensed features. Make those assumptions explicit in the capacity model.

## 4. Bare-metal infrastructure and installation

| Area | Minimum standard | Evidence |
|---|---|---|
| Hardware | MUST use vendor-supported server models and firmware. MUST document CPU, RAM, boot mode, disk layout, NICs, HBA/RAID mode, firmware versions, and warranty/support status for every node. | Hardware inventory and compatibility record |
| Out-of-band access | MUST protect BMC/IPMI/Redfish access on a management network, with named accounts, unique credentials, and audit where available. | Management network and access-control record |
| Power and facilities | MUST document redundant power feeds, rack placement, cooling capacity, and facility dependencies for the cluster. | Data-center design record |
| Installation method | MUST select and document an OCP-supported bare-metal installation method (for example, installer-provisioned, agent-based, or user-provisioned) and follow the matching release documentation end to end. | Install configuration, automation, and runbook |
| Provisioning | MUST define ownership and lifecycle for DHCP/PXE or provisioning services, firmware settings, boot media, and node replacement. Remove temporary bootstrap/provisioning capacity as required by the selected method. | Provisioning runbook and cleanup evidence |
| Configuration as code | SHOULD store reproducible installation inputs and platform configuration in version control. Secrets and private keys MUST be kept in an approved secrets system, not committed to Git. | Repository and secret-handling review |

## 5. Network, DNS, and ingress

| Area | Minimum standard | Evidence |
|---|---|---|
| Network plan | MUST record node, machine, pod, service, and external address ranges. Ranges MUST be non-overlapping with connected networks, VPNs, and planned secondary networks. | IPAM-approved network plan |
| Segmentation | MUST identify and restrict management, provisioning, control-plane, worker, storage, and application traffic according to the design. | VLAN/VRF and firewall rule matrix |
| Firewall | MUST permit only documented required flows between nodes and dependencies. Validate the port and protocol matrix against the selected OCP version and enabled features. | Reviewed firewall matrix and connectivity checks |
| DNS | MUST have forward and reverse resolution for nodes as required by the installation method, API endpoints, and application ingress. Required records MUST be in place before installation and tested from the relevant networks. | DNS record list and lookup results |
| Load balancing | MUST provide highly available API and application ingress load balancing. Health checks, listener ports, backends, failover behavior, and source-IP behavior MUST match the selected architecture and OCP documentation. | Load-balancer configuration and failover evidence |
| MTU and routing | MUST validate end-to-end MTU, routing, and asymmetric-path behavior for node, pod, storage, and external paths. | Network validation results |
| Network policy | MUST define an application namespace network-policy baseline and a process for controlled exceptions. | Policy examples and exception record |
| Egress | MUST document DNS, proxy, registry, update, telemetry, and application egress dependencies. Restrict egress to approved destinations where required by policy. | Egress allowlist/proxy configuration |

The specific load-balancer design and required ports depend on the installation method and release. Use the matching Red Hat installation guide rather than copying a port list from another architecture.

## 6. Storage and stateful workloads

| Area | Minimum standard | Evidence |
|---|---|---|
| Storage platform | MUST use a storage solution supported for the selected OCP release and workload. Document its failure domains, replication, performance, encryption, capacity, and vendor support model. | Storage design and compatibility record |
| Storage classes | MUST define and publish named StorageClasses with clear performance, access-mode, reclaim, encryption, and backup semantics. Set a default class only when one default is appropriate. | StorageClass catalog |
| Availability | MUST explain the impact of a node, controller, network, or array failure on each class. Local storage MUST be identified as node-bound and MUST have a workload recovery plan for node or disk loss. | Failure-mode and recovery matrix |
| Data protection | MUST include application-consistent backup and restore for persistent data. CSI snapshots alone MUST NOT be treated as a complete backup unless the solution copies protected data to an independent failure domain and the restore path has been tested. | Backup policy and restore test |
| Capacity and performance | MUST monitor capacity, latency, IOPS/throughput, and storage health. Define alert thresholds and expansion ownership. | Dashboards, alerts, and capacity runbook |

## 7. Identity, access, and security

| Area | Minimum standard | Evidence |
|---|---|---|
| Authentication | MUST integrate with an approved organizational identity provider. MUST define and test a controlled break-glass administrator process. | Identity-provider configuration and break-glass test |
| Authorization | MUST use least-privilege RBAC, group-based access where practical, named role owners, and periodic access reviews. Routine users and automation MUST NOT use cluster-admin. | Role bindings and access review |
| Workload isolation | MUST place applications in dedicated projects/namespaces with resource quotas and limit ranges appropriate to the service. Do not run customer workloads in platform-reserved namespaces. | Namespace baseline and admission checks |
| Pod security | Workloads MUST use the most restrictive compatible SCC and Pod Security Admission configuration. Privileged access, host networking, host paths, added capabilities, and host PID/IPC MUST require documented justification and approval. | Security exception register and policy evidence |
| Service accounts | MUST use dedicated service accounts with only the permissions required. Avoid automatically mounting API tokens when workloads do not need them. | Service-account/RBAC review |
| Secrets | MUST protect secrets at rest and in transit, restrict access, rotate credentials, and keep secret values out of source control and build logs. External secret management SHOULD be used where available. | Encryption configuration and rotation procedure |
| Images and supply chain | MUST use trusted registries and define image provenance, vulnerability remediation, and promotion controls. Production images SHOULD be immutable or pinned by digest. | Registry policy, scan results, and promotion workflow |
| Audit | MUST retain security-relevant API audit events and provide an approved retention and review path outside the cluster. Retention must meet organizational policy. | Audit configuration, destination, and retention |
| Vulnerability response | MUST have a process to review Red Hat advisories and remediate affected platform and workload components within policy-defined timelines. | Patch/remediation policy and sample record |

Do not weaken default platform security controls globally to accommodate an application. Record the narrowest exception, owner, expiry date, and compensating controls.

## 8. Workload reliability and resource governance

| Area | Minimum standard | Evidence |
|---|---|---|
| Scheduling | Critical stateless workloads MUST use multiple replicas and appropriate topology spread or anti-affinity. Stateful workloads MUST define their recovery and rescheduling behavior. | Workload deployment standards |
| Disruptions | Production workloads SHOULD define PodDisruptionBudgets that allow planned maintenance while not blocking critical platform operations. Validate their interaction with replica count and rollout strategy. | PDB review |
| Health and rollout | MUST define readiness and liveness probes where applicable, startup probes for slow-starting applications, and safe rollout/rollback procedures. | Deployment template and release runbook |
| Resources | MUST set resource requests for schedulable production workloads. Limits and quotas MUST be selected based on workload behavior and performance testing. | Resource policy and capacity review |
| Autoscaling | If autoscaling is used, MUST define minimum/maximum bounds, metrics dependencies, and behavior during metric or node-capacity failure. | Autoscaler configuration and test |
| Service objectives | Each production service MUST document owner, criticality, availability target, dependencies, maintenance impact, and RTO/RPO. | Service catalog entry |

## 9. Observability and incident response

| Area | Minimum standard | Evidence |
|---|---|---|
| Platform monitoring | MUST preserve and monitor the supported platform monitoring stack. MUST route actionable platform alerts to a staffed response path and document severity and escalation. | Alert routes and on-call runbook |
| Workload monitoring | MUST define how workload metrics and alerts are collected. Enable user-workload monitoring or an approved equivalent when teams need cluster-managed metrics and alerts. | Monitoring design and sample alert |
| Logging | MUST collect platform, audit, and application logs needed for operations and investigations. Logs required for incident response MUST be forwarded to durable storage outside the cluster with access and retention controls. | Log pipeline and retention evidence |
| Alert quality | MUST assign an owner and response action to each production alert. Test delivery of critical alerts. | Alert catalog and notification test |
| Runbooks | MUST maintain runbooks for common incidents, node maintenance, failed upgrades, storage incidents, certificate/identity issues, backup restore, and vendor escalation. | Version-controlled runbooks |
| Diagnostics | MUST document how to collect OCP diagnostics and protect diagnostic bundles as potentially sensitive data. | Diagnostic procedure |

## 10. Backup, disaster recovery, and availability

| Area | Minimum standard | Evidence |
|---|---|---|
| etcd | MUST schedule and protect etcd backups using the procedure for the exact OCP release. Store copies outside the cluster and outside its primary failure domain. | Backup job, protected location, and restore instructions |
| Cluster configuration | MUST version-control installation inputs and declarative platform configuration, excluding secret values. Document how to rebuild the cluster and restore operators and configuration. | Git repositories and rebuild procedure |
| Application data | MUST define application-level data and configuration backups, retention, immutability/access controls, and owners. | Backup matrix |
| Recovery objectives | MUST set approved RTO and RPO for the cluster and each critical service. | Business-approved objectives |
| Restore testing | MUST test cluster recovery and application data restoration at least annually and after major backup architecture changes. Record elapsed recovery time, data point restored, gaps, and actions. | Restore test report |
| Failure exercises | SHOULD exercise the loss of a worker, a control-plane member, a storage path, and an ingress/API dependency using a safe test plan. | Exercise records |

An etcd backup protects cluster state; it does not back up persistent application data. Follow Red Hat’s release-specific restore restrictions, including the requirement to use a backup from the matching z-stream version when applicable.

## 11. Change, upgrade, and maintenance management

1. Assign an owner and risk classification to each platform change.
2. Review Red Hat advisories, release notes, known issues, Operator compatibility, and the supported update path before scheduling an update.
3. Confirm cluster health, capacity, backups, storage health, PDB behavior, and support coverage.
4. Test changes in a representative non-production environment where available.
5. Notify service owners, agree on the maintenance window, and confirm business impact and rollback/recovery actions.
6. Apply supported updates using the OCP update mechanism and supported channel. Do not treat cluster downgrade as a rollback plan.
7. Monitor cluster operators, nodes, workloads, and alerts through completion; record the result and follow-up actions.

MUST maintain a regular patch and minor-version update cadence that keeps the cluster within support. MUST define maintenance ownership and a process to manage node drains and workload disruption. Use MachineConfig and supported Operators for node configuration; avoid unsupported manual host changes.

## 12. GitOps and configuration management

- MUST keep desired platform and application state reproducible, reviewed, and version-controlled.
- SHOULD use a supported GitOps controller such as Red Hat OpenShift GitOps where it fits the operating model.
- MUST require peer review and automated validation for changes to cluster-scoped resources, RBAC, network policy, storage, and security settings.
- MUST separate secrets from plain-text manifests and control who can modify production branches and release approvals.
- MUST define drift detection, reconciliation ownership, emergency changes, and how emergency changes are returned to Git.
- MUST retain a known-good configuration revision and document the recovery process.

## 13. Production acceptance checklist

Complete this checklist before handover:

- [ ] Supported OCP release, architecture, hardware, and Operators verified.
- [ ] Design, topology, capacity, failure domains, network, DNS, load balancing, and storage documented.
- [ ] Three control-plane nodes present for the standard HA profile; worker failure capacity demonstrated.
- [ ] Cluster Operators are healthy and all nodes are Ready after installation or change.
- [ ] Identity integration, least-privilege access, break-glass access, audit retention, and security exceptions reviewed.
- [ ] Storage classes, encryption, backup coverage, and application restore tested.
- [ ] Platform and workload alert routing, log forwarding, retention, and on-call ownership verified.
- [ ] Upgrade procedure, maintenance window, application disruption controls, and support path documented.
- [ ] RTO/RPO approved; etcd and application backup recovery procedures tested.
- [ ] Git repositories, runbooks, inventory, service owners, and escalation contacts handed over.
- [ ] Open risks and exceptions have an owner, approver, mitigation, expiry/review date, and tracking reference.

### Suggested post-install health snapshot

Run these commands with an authorized account and attach the output, or an appropriately redacted summary, to the acceptance record:

~~~bash
oc get clusterversion
oc get clusteroperators
oc get nodes -o wide
oc get machineconfigpools
oc get storageclass
oc get pods -A
oc get events -A --sort-by=.lastTimestamp
~~~

A green snapshot is only point-in-time evidence. It does not replace a backup restore test, capacity model, or workload-level validation.

## 14. Required project decisions

The project owner must fill these values before this standard is applied to a specific environment:

| Decision | Selected value / reference |
|---|---|
| OCP version and update channel | TBD |
| Bare-metal installation method | TBD |
| Site, racks, power, and failure domains | TBD |
| Control-plane and worker inventory | TBD |
| Node, pod, service, and external network ranges | TBD |
| DNS, NTP, proxy, firewall, and load-balancer owners | TBD |
| Storage backend, classes, and backup integration | TBD |
| Identity provider and break-glass owner | TBD |
| Monitoring, logging, alert destination, and retention | TBD |
| Backup location, RTO, RPO, and restore test owner | TBD |
| Maintenance window and upgrade owner | TBD |
| Application service owners and support contacts | TBD |
| Risk/exception approval authority | TBD |

## 15. References

Use the documentation matching the deployed OCP release. The links below point to Red Hat’s OCP 4.22 documentation as a current reference set for this draft; replace or verify them when the target release is selected.

1. [Installing on bare metal](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/installing_on_bare_metal/)
2. [OpenShift Container Platform 4.22 release notes](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/release_notes/)
3. [OpenShift Container Platform life cycle policy](https://access.redhat.com/support/policy/updates/openshift)
4. [Updating clusters](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/updating_clusters/)
5. [Backing up and restoring etcd](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/etcd/backing-up-and-restoring-etcd-data)
6. [Backup and restore overview](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/backup_and_restore/)
7. [Authentication and authorization](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/authentication_and_authorization/)
8. [Security and compliance](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/security_and_compliance/)
9. [Storage](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/storage/)
10. [Networking overview](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/networking_overview/)
11. [Network security](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/network_security/)
12. [Machine configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/machine_configuration/)
13. [Monitoring stack for Red Hat OpenShift](https://docs.redhat.com/en/documentation/monitoring_stack_for_red_hat_openshift/4.22/)
14. [OpenShift Operator life cycles](https://access.redhat.com/support/policy/updates/openshift_operators)
