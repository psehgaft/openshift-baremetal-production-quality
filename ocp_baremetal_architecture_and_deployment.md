# OCP Bare-Metal Architecture and Deployment Record

> **Purpose:** Fillable architecture description and deployment record for a production OpenShift Container Platform (OCP) bare-metal cluster installed with the web-based Assisted Installer.  
> **How to use:** Copy this file for each environment, replace every {{PLACEHOLDER}}, complete each checklist, attach approved diagrams and sanitized screenshots, and store the final document with the change and installation evidence.  
> **Document ID:** {{DOCUMENT_ID}}  
> **Version:** {{DOCUMENT_VERSION}}  
> **Status:** {{DRAFT | FOR REVIEW | APPROVED | AS-BUILT}}  
> **Cluster / environment:** {{CLUSTER_NAME}} / {{ENVIRONMENT}}  
> **Owner:** {{DOCUMENT_OWNER}}  
> **Last updated:** {{YYYY-MM-DD}}

---

## Document control

| Field | Value |
|---|---|
| Business/service owner | {{BUSINESS_SERVICE_OWNER}} |
| Platform owner | {{PLATFORM_OWNER}} |
| Architecture owner / approver | {{ARCHITECT_NAME_AND_APPROVER}} |
| Deployment lead | {{DEPLOYMENT_LEAD}} |
| Security reviewer | {{SECURITY_REVIEWER}} |
| Change / implementation ticket | {{CHANGE_ID}} |
| Repository and commit / release tag | {{REPOSITORY_URL_AND_COMMIT}} |
| Maintenance window | {{WINDOW_WITH_TIMEZONE}} |
| OCP version and channel | {{OCP_VERSION_AND_CHANNEL}} |
| Assisted Installer documentation reviewed | {{REVIEW_DATE}} |
| Related production standard | [OpenShift Bare-Metal Production Standard](./OPENSHIFT_PRODUCTION_STANDARD.md) |

### Revision history

| Version | Date | Author | Change | Approval |
|---|---|---|---|---|
| {{0.1}} | {{YYYY-MM-DD}} | {{NAME}} | Initial deployment plan / as-built record | {{PENDING}} |

### Fill-in and image instructions

- Replace every {{...}} value. If not applicable, enter N/A with a reason; do not leave an unexplained blank.
- Complete design fields before creating the Assisted Installer cluster. During deployment, record actual values and deviations in the as-built fields.
- Save screenshots and diagrams next to this document under images/{{CLUSTER_NAME}}/. Use numbered filenames such as 01-cluster-details.png, 02-hosts-discovered.png, and 03-preinstall-validation.png.
- At each image marker, insert Markdown image syntax with a relative link, for example: ![Cluster details](./images/cluster-name/02-cluster-details.png). Record capture date, step, and owner in the evidence index.
- Capture only what is needed. Redact access tokens, passwords, private keys, kubeconfig content, BMC credentials, personal data, and sensitive customer details before saving or sharing images.
- Assisted Installer UI labels can change between releases. Follow the exact-release Red Hat documentation in Section 14; if a control differs, record its displayed label and approved choice.

## 1. Executive summary

| Field | Planned value | As-built value |
|---|---|---|
| Cluster name | {{CLUSTER_NAME}} | {{AS_BUILT}} |
| Purpose / workload class | {{PURPOSE}} | {{AS_BUILT}} |
| Environment | {{DEV | TEST | STAGING | PRODUCTION}} | {{AS_BUILT}} |
| Site / data center | {{SITE}} | {{AS_BUILT}} |
| Region / jurisdiction | {{REGION_AND_JURISDICTION}} | {{AS_BUILT}} |
| Installation method | Web-based Assisted Installer | {{AS_BUILT}} |
| OCP version / channel | {{VERSION_AND_CHANNEL}} | {{AS_BUILT}} |
| Cluster profile | {{HA_MULTI_NODE | OTHER_APPROVED_PROFILE}} | {{AS_BUILT}} |
| Node count | {{CONTROL_PLANE_COUNT}} control-plane + {{WORKER_COUNT}} workers | {{AS_BUILT}} |
| CNI / network type | {{OVN_KUBERNETES_AND_NETWORKING_MODE}} | {{AS_BUILT}} |
| Storage profile | {{STORAGE_PROFILE}} | {{AS_BUILT}} |
| API endpoint / VIP | {{API_FQDN_AND_VIP}} | {{AS_BUILT}} |
| Ingress endpoint / VIP | {{INGRESS_FQDN_AND_VIP}} | {{AS_BUILT}} |
| RTO / RPO | {{APPROVED_RTO_AND_RPO}} | {{AS_BUILT}} |
| Deployment start / end | {{START_TIME}} / {{END_TIME}} | {{AS_BUILT}} |
| Acceptance decision | {{PENDING}} | {{APPROVED | REJECTED | CONDITIONAL}} |

### 1.1 Scope and exclusions

**In scope:** {{CLUSTER_COMPONENTS_AND_SERVICES}}  
**Out of scope:** {{EXCLUSIONS_AND_REASON}}  
**Assumptions:** {{ASSUMPTIONS}}  
**Known risks / constraints:** {{RISKS_AND_CONSTRAINTS}}

> This record describes a deployment and architecture; it does not certify ISO conformity. Use the production standard for control status, evidence, and approval requirements.

## 2. Stakeholders and responsibilities

| Role | Name / group | Responsibility | Contact / escalation | Approval required |
|---|---|---|---|---|
| Business / cloud service customer | {{OWNER}} | Service requirements, data classification, workload RTO/RPO and acceptance | {{CONTACT}} | {{YES/NO}} |
| Cloud service provider / platform owner | {{OWNER}} | OCP design, build, security baseline, support, lifecycle, operations | {{CONTACT}} | {{YES/NO}} |
| Deployment lead | {{OWNER}} | Installation coordination, evidence, change record | {{CONTACT}} | {{YES/NO}} |
| Network / carrier team | {{OWNER}} | VLANs, routing, DNS, NTP, firewall, load balancer | {{CONTACT}} | {{YES/NO}} |
| Server / facilities partner | {{OWNER}} | Hardware, firmware, BMC, rack, power, cooling | {{CONTACT}} | {{YES/NO}} |
| Storage / backup partner | {{OWNER}} | Storage services, CSI, data protection, restore | {{CONTACT}} | {{YES/NO}} |
| Identity / security team | {{OWNER}} | IdP, privileged access, certificates, security review | {{CONTACT}} | {{YES/NO}} |
| Auditor / assessor | {{OWNER_OR_N/A}} | Review criteria and evidence, if in scope | {{CONTACT}} | {{YES/NO}} |
| Application owners | {{OWNER}} | Workload readiness, tests, service-level validation | {{CONTACT}} | {{YES/NO}} |

### 2.1 Shared responsibility

| Control area | Platform provider | Customer / application team | Partner / third party | Evidence |
|---|---|---|---|---|
| Physical security and facilities | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |
| Server firmware / BMC / replacement | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |
| Network / DNS / load balancing | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |
| OCP lifecycle / cluster security | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |
| Workload configuration / image security | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |
| Persistent data backup / restore | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |
| Monitoring / incident response | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{RESPONSIBILITY}} | {{EVIDENCE}} |

## 3. Architecture description

Complete this section before installation, then revise it to match the deployed environment.

### 3.1 Service and deployment model

- Customer-visible service model: {{CONTAINER_PLATFORM_SERVICE | OTHER}}
- Deployment model and justification: {{PRIVATE_CLOUD | OTHER | EXPLAIN}}
- Service catalog / tier: {{SERVICE_NAME_AND_TIER}}
- Availability objective: {{SLO_OR_SLA}}
- Support hours and escalation: {{DETAILS}}
- Data classifications permitted: {{CLASSIFICATIONS}}
- Tenant / namespace model: {{MODEL}}
- Service boundary: {{IN_SCOPE_COMPONENTS}}
- External dependencies: {{DEPENDENCIES}}

### 3.2 Architecture views

| View ID | Required view | Diagram / source link | Version | Owner |
|---|---|---|---|---|
| V-CTX | Context, roles, service boundary, external dependencies | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-FUNC | Functional components and platform services | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-DEP | Physical deployment, racks, nodes, network, load balancers, storage | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-SEC | Trust boundaries, identity flows, ingress/egress, privileged paths | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-DATA | Data classification, persistence, backup and restore flows | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-OPS | Provisioning, observability, change, incident and support flows | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-RES | Failure domains, dependencies, RTO/RPO, recovery sequence | {{LINK}} | {{VERSION}} | {{OWNER}} |
| V-LCM | GitOps, upgrades, Operators, drift management and retirement | {{LINK}} | {{VERSION}} | {{OWNER}} |

**Architecture decision records:** {{ADR_LINKS}}  
**Assumptions, limitations, and unresolved decisions:** {{DETAILS}}

### 3.3 Physical topology and node inventory

> Example planning profile: 3 control-plane nodes and 6 workers (9 total) is a project starting point, not a universal minimum. Confirm capacity, failure domains, OCP support, and workload requirements before adopting it.

| Hostname | Role | Manufacturer / model | Serial / asset ID | BMC reference (restricted) | Rack / power domain | CPU / RAM | Boot / install disk | NIC names / MACs | OCP node name |
|---|---|---|---|---|---|---|---|---|---|
| {{HOST-01}} | {{CONTROL PLANE}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-02}} | {{CONTROL PLANE}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-03}} | {{CONTROL PLANE}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-04}} | {{WORKER}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-05}} | {{WORKER}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-06}} | {{WORKER}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-07}} | {{WORKER}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-08}} | {{WORKER}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |
| {{HOST-09}} | {{WORKER}} | {{MODEL}} | {{ASSET}} | {{VAULT_REF}} | {{RACK/PDU}} | {{CPU/RAM}} | {{DISK}} | {{NIC/MAC}} | {{NODE}} |

**Firmware / BIOS baseline:** {{VERSION_AND_SETTINGS_REFERENCE}}  
**BMC vault reference (no secrets):** {{VAULT_REFERENCE}}  
**Hardware compatibility evidence:** {{LINK_OR_TICKET}}  
**Capacity / N+1 calculation:** {{LINK_OR_SUMMARY}}

### 3.4 Network plan

Fill with IPAM-approved values. Check all ranges for overlap with connected networks, VPNs, storage, and future expansion.

| Network / setting | Planned value | As-built value | Owner / evidence |
|---|---|---|---|
| Base DNS domain | {{BASE_DOMAIN}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| Cluster name / FQDN | {{CLUSTER_NAME}} / {{CLUSTER_FQDN}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| Machine network CIDR | {{MACHINE_CIDR}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| Pod network CIDR / host prefix | {{POD_CIDR}} / {{HOST_PREFIX}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| Service network CIDR | {{SERVICE_CIDR}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| API VIP / FQDN | {{API_VIP}} / {{API_FQDN}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| Ingress VIP / wildcard DNS | {{INGRESS_VIP}} / {{WILDCARD_FQDN}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| DNS resolvers / records | {{DNS_SERVERS_AND_RECORDS}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| NTP sources | {{NTP_SERVERS}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| VLAN / VRF / routes | {{DETAILS}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| Firewall / proxy / egress | {{REFERENCE}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| MTU (machine / storage / overlay) | {{VALUES}} | {{VALUE}} | {{OWNER / EVIDENCE}} |
| API and ingress load-balancing mode | {{CLUSTER-MANAGED | USER-MANAGED}} | {{VALUE}} | {{OWNER / EVIDENCE}} |

**Network management mode:** {{CLUSTER-MANAGED | USER-MANAGED | OTHER_SUPPORTED_MODE}}  
**Network interface plan:** NIC1 {{ROLE}}; NIC2 {{ROLE}}; bond/VLAN/MTU {{DETAILS}}  
**MAC-to-interface inventory source:** {{REFERENCE}}  
**IPv4 / dual-stack:** {{CHOICE_AND_RATIONALE}}  
**Separate storage network:** {{YES/NO/N/A_WITH_REASON}}  
**External load balancers:** {{TYPE, VIPS, PORTS, HEALTH CHECKS, OWNERS}}  
**Connectivity test evidence:** {{LINK}}

### 3.5 Storage and data-protection plan

| Storage use | Backend / CSI driver | StorageClass | Access mode | Encryption | Replication / failure domains | Backup / restore |
|---|---|---|---|---|---|---|
| OCP node / etcd disks | {{LOCAL_DISK}} | N/A | N/A | {{DETAILS}} | {{DETAILS}} | Release-specific etcd backup |
| Application block | {{BACKEND}} | {{CLASS}} | {{RWO}} | {{DETAILS}} | {{DETAILS}} | {{METHOD}} |
| Application file/shared | {{BACKEND}} | {{CLASS}} | {{RWX}} | {{DETAILS}} | {{DETAILS}} | {{METHOD}} |
| Local ephemeral / local PV, if used | {{BACKEND}} | {{CLASS}} | {{MODE}} | {{DETAILS}} | Node-bound | {{NODE-LOSS-RECOVERY}} |

**Local Storage Operator (LSO) required?** {{YES/NO}}  
**If yes, record use, devices, discovery rules, node affinity, capacity, failure impact, and recovery:** {{DETAILS}}  
**Restore test date, RPO achieved, RTO achieved:** {{LINK_AND_RESULT}}

### 3.6 Security and operations plan

- Identity provider / group mapping: {{REFERENCE}}
- Privileged and break-glass process: {{VAULT_AND_RUNBOOK_REFERENCE}}
- RBAC model / cluster-admin owners: {{REFERENCE}}
- SCC / Pod Security / admission baseline: {{REFERENCE}}
- Secrets, certificate authority, KMS / encryption owners: {{REFERENCE}}
- Image registry, scanning, provenance and promotion: {{REFERENCE}}
- Audit destination and retention: {{REFERENCE}}
- Platform/workload monitoring and alert destination: {{REFERENCE}}
- Central logging destination and retention: {{REFERENCE}}
- GitOps/configuration repositories and approval policy: {{REFERENCE}}
- Backup destination, immutability, access controls: {{REFERENCE}}
- Incident, change, upgrade, maintenance, support runbooks: {{REFERENCE}}

### 3.7 Service Mesh / Sidecar design

Complete this section when the cluster will run a service mesh. If the answer is no, record the rationale and future trigger.

**Service mesh required?** {{YES/NO}}  
**Use case / workloads:** {{SERVICE-TO-SERVICE_IDENTITY | MTLS | TRAFFIC_POLICY | TELEMETRY | OTHER}}  
**Selected product/version and OCP compatibility evidence:** {{PRODUCT_VERSION_AND_REFERENCE}}  
**Production feature status:** {{GA_AND_SUPPORTED | NOT_APPROVED_FOR_PRODUCTION}}  
**Mode:** {{SIDE-CAR | OTHER_SUPPORTED_MODE}}  
**Control-plane namespace / Istio resource:** {{VALUES}}  
**IstioRevision and update strategy:** {{REVISION / IN-PLACE_OR_REVISION-BASED}}  
**Application namespaces/workloads in scope:** {{NAMESPACES_AND_SERVICES}}  
**Excluded namespaces/workloads and rationale:** {{DETAILS}}  
**Injection control:** {{NAMESPACE_LABEL | POD_TEMPLATE_LABEL}}  
**Source of truth for labels/custom resources:** {{GITOPS_PATH}}  
**Ingress/egress gateways and external dependencies:** {{DETAILS}}  
**mTLS target, workload authorization, certificate issuer/rotation:** {{POLICY_REFERENCE}}  
**NetworkPolicy/firewall/API-gateway interaction:** {{DESIGN_REFERENCE}}  
**Proxy CPU/memory/ephemeral resource budget:** {{VALUES_AND_TEST}}  
**PDB, probes, graceful termination, topology spread:** {{REFERENCE}}  
**Mesh metrics/logs/alerts, retention and access policy:** {{REFERENCE}}  
**Upgrade, rollback, onboarding and decommission runbooks:** {{REFERENCE}}

Injection is applied when pods are created. Plan a controlled rollout after changing namespace membership or revision labels, then verify the injected proxy in each workload pod. Do not enable injection for all namespaces by default. Service Mesh does not replace OpenShift NetworkPolicy, perimeter firewalling, an API gateway, or application authorization.

Supported illustrative checks:

~~~bash
oc get istio -A
oc get istiorevisions
oc describe pod <pod-name> -n <application-namespace>
~~~

Example for an approved non-default revision (confirm exact value and supported label syntax in the installed product guide):

~~~bash
oc label namespace <application-namespace> istio.io/rev=<revision-name>
~~~

**Image slot — service mesh architecture and trust boundaries**  
<!-- Insert: ![Service mesh architecture](./images/{{CLUSTER_NAME}}/mesh-architecture.png) -->

**Image slot — injected sidecar and revision verification**  
<!-- Insert: ![Injected sidecar](./images/{{CLUSTER_NAME}}/mesh-sidecar-verification.png) -->

### 3.8 Common Operator selection and inventory

Operators are not all equivalent. Distinguish CVO-managed OpenShift platform operators from optional add-on Operators managed through OLM/OperatorHub. This catalog is a decision aid, not a deployment mandate. Confirm the target release, architecture, support state, entitlement, catalog source, dependencies, and installation method before selecting an Operator. Do not install an option for production if the current installation workflow marks it Developer Preview or Technology Preview.

| Operator / capability | Use case | Needed? | Install phase | Version/channel | Namespace/scope | Owner / support |
|---|---|---|---|---|---|---|
| Core ClusterOperators managed by CVO | Cluster platform functions; inventory only | {{N/A - PLATFORM MANAGED}} | Installed with OCP | {{RELEASE}} | Cluster | {{PLATFORM OWNER}} |
| OpenShift GitOps | Declarative deployment / drift reconciliation | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| OpenShift Pipelines | Tekton CI/CD | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| OpenShift Logging | Log collection/forwarding | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| OpenShift Service Mesh | Service identity, mTLS, traffic policy, telemetry | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| ODF | Software-defined storage | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| Local Storage Operator (LSO) | Local persistent volumes | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| Logical Volume Manager Storage | Local LVM-backed storage | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| Kubernetes NMState | Day-2 node networking | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| MetalLB | LoadBalancer IPs on bare metal | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| OADP | Backup integration | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| Compliance Operator | Compliance scanning | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| cert-manager | Workload certificate lifecycle | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| OpenShift Virtualization | VM workloads | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| NFD / SR-IOV Network Operator | Hardware discovery / high-performance networking | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |
| Other vendor/community Operator | {{USE CASE}} | {{YES/NO}} | {{INSTALL/DAY 2}} | {{VALUE}} | {{VALUE}} | {{OWNER}} |

For every selected Operator, document its publisher/catalog, exact release compatibility, required permissions and CRDs, resource/capacity impact, dependencies, upgrade approval strategy, backup/export and removal behavior, monitoring, and support contacts. Add the architecture decision and change record here: {{LINKS}}.

**Assisted Installer optional selection decision:** {{LIST_SELECTED_AND_DECLINED}}  
**If selecting via Assisted Installer:** confirm the current UI status for the exact release and platform; Developer Preview/Technology Preview selections are not approved for production.  
**Image slot — Operator selection / subscription inventory**  
<!-- Insert: ![Operator inventory](./images/{{CLUSTER_NAME}}/operator-inventory.png) -->

## 4. Installation prerequisites and go/no-go

Complete and approve before creating the cluster in the Assisted Installer.

### 4.1 Subscription and access

- [ ] Red Hat account and active OCP subscription available to the deployment team.
- [ ] Hybrid Cloud Console access approved and tested.
- [ ] Platform/infrastructure support entitlements and contacts recorded.
- [ ] Installation environment has required outbound connectivity, or a supported disconnected procedure is documented.
- [ ] Change record, maintenance window, business notification, and approvers confirmed.
- [ ] No credentials, tokens, pull secrets, or private keys stored in this document or screenshots.

**Organization / subscription reference (non-secret):** {{REFERENCE}}  
**Console access owner:** {{OWNER}}  
**Change record:** {{CHANGE_ID}}

### 4.2 Infrastructure readiness

- [ ] Node inventory matches approved hardware and capacity design.
- [ ] BIOS/UEFI, CPU features, boot mode, NIC firmware, RAID/HBA mode, disks, and firmware match vendor/OCP support requirements.
- [ ] Each node has stable hostname/IP or approved DHCP reservation; MAC addresses verified against inventory.
- [ ] Installation disks identified; data on selected target disks backed up or approved for destruction.
- [ ] BMC access tested and restricted to the management network.
- [ ] Rack, redundant power, cooling, cable map, and failure domains verified.
- [ ] USB, Redfish virtual media, or approved network boot method available for the discovery image.
- [ ] Time source and clock synchronization verified.
- [ ] Storage backend and supported CSI integration ready if needed for initial workloads.
- [ ] Hardware support contacts and replacement procedure known.

**Firmware baseline:** {{LINK}}  
**Disk destruction approval:** {{TICKET_AND_APPROVER}}  
**BMC/network readiness:** {{LINK}}

### 4.3 Network and DNS readiness

- [ ] Address ranges approved and non-overlapping.
- [ ] Required DNS records prepared for API, API-int where applicable, wildcard application ingress, and nodes according to the selected design.
- [ ] Reverse DNS prepared where required by the design.
- [ ] API and ingress VIPs reserved and reachable.
- [ ] External load balancers provisioned and health checks ready if using user-managed networking.
- [ ] Cluster-managed networking selected only if supported for this platform, architecture, topology, and release.
- [ ] DHCP scope, reservations, gateway, DNS, and NTP options tested if DHCP is used.
- [ ] Firewall and proxy allow required endpoints and protocols for the exact release.
- [ ] MTU and VLAN trunking verified end to end.
- [ ] Every NIC MAC, switch port, VLAN, and intended role recorded.

**DNS test evidence:** {{LINK}}  
**Load-balancer test evidence:** {{LINK}}  
**Firewall/proxy approval:** {{CHANGE_OR_TICKET}}  
**Network owner sign-off:** {{NAME_AND_DATE}}

### 4.4 Go/no-go approval

| Gate | Result | Evidence / ticket | Approver | Date |
|---|---|---|---|---|
| Hardware/support readiness | {{PASS/FAIL}} | {{LINK}} | {{NAME}} | {{DATE}} |
| Network/DNS/load-balancer readiness | {{PASS/FAIL}} | {{LINK}} | {{NAME}} | {{DATE}} |
| Storage/data-protection readiness | {{PASS/FAIL}} | {{LINK}} | {{NAME}} | {{DATE}} |
| Security/access readiness | {{PASS/FAIL}} | {{LINK}} | {{NAME}} | {{DATE}} |
| Change/business approval | {{PASS/FAIL}} | {{LINK}} | {{NAME}} | {{DATE}} |
| Go/no-go decision | {{GO/NO-GO}} | {{RECORD}} | {{CHAIR}} | {{DATE/TZ}} |

Do not proceed while a required gate fails or a blocking dependency has no approved mitigation.

## 5. Assisted Installer procedure

> The Hosted Assisted Installer is a web service in the Red Hat Hybrid Cloud Console. The workflow creates a cluster configuration, generates a discovery image, boots hosts so they can report hardware and network data, runs validations, and then installs OCP. Bare-metal infrastructure, networking, load balancing, storage, and machines remain customer-provided and customer-operated.

### Step 1 — Confirm installation mode and OCP release

1. Confirm this is the web-based Assisted Installer workflow, not the local Agent-based Installer.
2. Confirm the network has the connectivity required for the hosted installer and selected OCP release.
3. Confirm the target OCP version, architecture, update channel, and support status.
4. Confirm topology: highly available multi-node, single-node, compact, dual-stack, or another supported profile.
5. Confirm DHCP or static host networking.
6. Confirm cluster-managed or user-managed networking.
7. Confirm Operators/bundles needed at installation (for example, ODF or OpenShift Virtualization); check exact-release and platform support.
8. Confirm whether extra manifests/patches are required and have been reviewed.
9. Decide which optional Operators are genuinely required. Confirm their supported status for the selected release and platform, dependencies, entitlement, install method, and day-2 owner. Some options may be better installed after cluster creation. Never select a Developer Preview or Technology Preview for production.

**Selected mode:** {{WEB_ASSISTED_INSTALLER}}  
**Release / architecture / topology:** {{VALUES}}  
**Support check:** {{LINK_OR_CASE}}  
**Decisions and rationale:** {{ADR_ID}}

**Image slot — installation approach / approved topology**  
<!-- Insert: ![Installation approach](./images/{{CLUSTER_NAME}}/01-installation-approach.png) -->

### Step 2 — Create the cluster configuration in the Hybrid Cloud Console

1. Sign in to the [Red Hat Hybrid Cloud Console](https://console.redhat.com/) with an approved account.
2. Navigate to Red Hat OpenShift / Create cluster in the current console menu.
3. Select Datacenter and the Assisted Installer workflow. Confirm the intended OCP version and platform.
4. Enter cluster name, base domain, target release/version, CPU architecture, and cluster type.
5. Select approved networking and optional product choices. Review the optional Operator/bundle page against Section 3.8; record selected and declined items. Bundle selection can install related Operators with additional dependencies.
6. Before choosing an Operator, verify the exact version, platform support, bundle contents, entitlement, and production support status in the current product documentation. If the installer marks an option Developer Preview or Technology Preview, do not select it for production; use a supported post-install procedure if available.
7. Record the UI summary and intended day-2 configuration. Advanced configuration may need to be applied after installation.
8. Review defaults. Compare each relevant value with the IPAM-approved plan and architecture decisions.
9. Record the cluster ID and creation timestamp. Do not record API tokens.

| Console field | Planned value | Actual value | Verified |
|---|---|---|---|
| Cluster name | {{CLUSTER_NAME}} | {{VALUE}} | {{YES/NO}} |
| Base domain | {{BASE_DOMAIN}} | {{VALUE}} | {{YES/NO}} |
| OCP version | {{VERSION}} | {{VALUE}} | {{YES/NO}} |
| CPU architecture | {{ARCH}} | {{VALUE}} | {{YES/NO}} |
| Cluster type | {{HA_MULTI_NODE}} | {{VALUE}} | {{YES/NO}} |
| Networking mode | {{MODE}} | {{VALUE}} | {{YES/NO}} |
| Optional Operators/bundles | {{LIST_OR_NONE}} | {{VALUE}} | {{YES/NO}} |
| Cluster ID | N/A | {{CLUSTER_ID}} | {{RECORDED}} |

**Created by / date / timezone:** {{NAME / TIMESTAMP}}  
**Cluster ID:** {{CLUSTER_ID}}  
**Image slot — Assisted Installer cluster details**  
<!-- Insert: ![Cluster details](./images/{{CLUSTER_NAME}}/02-cluster-details.png) -->

### Step 3 — Configure cluster networking

1. Select the approved network management mode:
   - Cluster-managed networking if its integrated API/Ingress VIP behavior matches the architecture.
   - User-managed networking if external load balancing/DNS or a non-standard/multi-subnet topology is required.
2. Select the supported cluster network type. OVN-Kubernetes is the standard default for current releases; verify the exact release. Do not select a Technology Preview or unsupported CNI for production.
3. Enter the machine network CIDR and any supported additional machine networks.
4. Enter API and ingress VIPs for cluster-managed networking, or record external VIP/load-balancer settings for user-managed networking.
5. Enter pod network CIDR/host prefix and service network CIDR if shown or required.
6. For dual-stack, confirm supported address families, primary/secondary order, subnet sizes, matching VIP families, routes, DNS, and firewall flows.
7. Compare values with Section 3.4 and IPAM approval; check for overlap.
8. If using HTTP/S proxy, enter approved proxy and no-proxy values. Do not put proxy credentials in this document.
9. Wait for validation; capture results and resolve blocking errors before continuing.

**Network mode / CNI:** {{VALUES}}  
**Machine / pod / service ranges:** {{VALUES}}  
**API / ingress VIPs:** {{VALUES}}  
**Proxy:** {{YES/NO; REFERENCE ONLY}}  
**Validation result:** {{PASS/FAIL; LINK}}

**Image slot — network settings and VIPs (redact sensitive details)**  
<!-- Insert: ![Network settings](./images/{{CLUSTER_NAME}}/03-network-settings.png) -->

### Step 4 — Generate and secure the discovery ISO

1. Review the cluster configuration summary and verify release and architecture.
2. Generate the discovery ISO in the Assisted Installer.
3. Download it to the approved staging host; record filename, timestamp, size, and SHA-256 checksum if available.
4. Store it in a controlled location. Do not commit the ISO to Git.
5. Choose and document USB, Redfish virtual media, or approved iPXE/network boot.
6. Verify boot order and remote console access on each target server.

| Artifact / action | Result |
|---|---|
| Discovery ISO filename | {{FILENAME}} |
| Download timestamp | {{YYYY-MM-DD HH:MM TZ}} |
| SHA-256 (if available) | {{CHECKSUM}} |
| Staging location reference | {{CONTROLLED_LOCATION_ID — NO PUBLIC URL}} |
| Boot method | {{USB | REDFISH VIRTUAL MEDIA | iPXE}} |
| Boot order / console test | {{PASS/FAIL; EVIDENCE}} |

**Image slot — discovery ISO ready**  
<!-- Insert: ![Discovery ISO ready](./images/{{CLUSTER_NAME}}/04-discovery-iso-ready.png) -->

### Step 5 — Boot and discover bare-metal hosts

Perform one host at a time or in controlled batches so each discovered host can be matched to the inventory.

1. Confirm target hostname, asset ID, BMC console, NIC MACs, and intended role against Section 3.3.
2. Boot the server using the discovery ISO. Do not manually select or wipe a disk unless approved procedure requires it.
3. Wait for the discovery agent to start and the host to appear in Assisted Installer.
4. Match displayed host to physical server using inventory and MACs. Set or confirm the hostname.
5. Repeat for every control-plane and worker host.
6. Confirm discovered host count equals approved inventory.
7. Investigate missing interfaces, duplicate MACs, unexpected disks, hardware warnings, or identity mismatches.
8. Record host IDs only if operationally needed; do not record secrets.

| Inventory hostname | Discovered hostname | Host ID (optional) | Expected role | Status | Issue / resolution |
|---|---|---|---|---|---|
| {{HOST-01}} | {{VALUE}} | {{VALUE}} | Control plane | {{STATUS}} | {{DETAILS}} |
| {{HOST-02}} | {{VALUE}} | {{VALUE}} | Control plane | {{STATUS}} | {{DETAILS}} |
| {{HOST-03}} | {{VALUE}} | {{VALUE}} | Control plane | {{STATUS}} | {{DETAILS}} |
| {{HOST-04}} | {{VALUE}} | {{VALUE}} | Worker | {{STATUS}} | {{DETAILS}} |
| {{HOST-05}} | {{VALUE}} | {{VALUE}} | Worker | {{STATUS}} | {{DETAILS}} |
| {{HOST-06}} | {{VALUE}} | {{VALUE}} | Worker | {{STATUS}} | {{DETAILS}} |
| {{HOST-07}} | {{VALUE}} | {{VALUE}} | Worker | {{STATUS}} | {{DETAILS}} |
| {{HOST-08}} | {{VALUE}} | {{VALUE}} | Worker | {{STATUS}} | {{DETAILS}} |
| {{HOST-09}} | {{VALUE}} | {{VALUE}} | Worker | {{STATUS}} | {{DETAILS}} |

**Image slot — discovered host inventory**  
<!-- Insert: ![Discovered hosts](./images/{{CLUSTER_NAME}}/05-hosts-discovered.png) -->

### Step 6 — Assign host roles and installation disks

1. Assign exactly the approved number of hosts to control-plane role. The standard HA profile normally uses three separate physical control-plane hosts.
2. Assign worker/compute roles to approved remaining hosts. Do not assume all discovered hosts should join.
3. Inspect disks for each host and select the installation target according to the design.
4. Verify selected disk by model, size, and device path against inventory. Installation overwrites the selected disk.
5. Do not select a data disk, local-storage device, or SAN/iSCSI device as the OS disk unless explicitly designed and approved.
6. Review optional disk encryption and supported disk configuration. Record selected option and key owner; never record key material.
7. If LSO/local storage is planned, record data devices separately and validate release-specific LSO guidance. LSO does not make a local disk shared or resilient to node loss.
8. Have a second operator review host-to-role and host-to-disk mapping.

| Hostname | Role | OS install disk | Data/local disks preserved | Second-person check |
|---|---|---|---|---|
| {{HOST-01}} | {{CONTROL PLANE}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-02}} | {{CONTROL PLANE}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-03}} | {{CONTROL PLANE}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-04}} | {{WORKER}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-05}} | {{WORKER}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-06}} | {{WORKER}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-07}} | {{WORKER}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-08}} | {{WORKER}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |
| {{HOST-09}} | {{WORKER}} | {{DEVICE/MODEL/SIZE}} | {{DETAILS}} | {{NAME/DATE}} |

**Image slot — roles and installation disks**  
<!-- Insert: ![Host role and disk mapping](./images/{{CLUSTER_NAME}}/06-host-roles-disks.png) -->

### Step 7 — Configure host networking

Choose the method matching the approved design.

#### Option A — DHCP

1. Verify the machine-network NIC is connected and active on every host.
2. Verify DHCP supplies intended address, gateway, DNS, and NTP options if used.
3. Confirm addresses shown in Assisted Installer match IPAM reservations.
4. Confirm multiple NICs do not cause the wrong default route or machine network to be selected.

**DHCP scope/reservation reference:** {{REFERENCE}}  
**Host address verification:** {{RESULT}}

#### Option B — Static host networking with NMState

1. Use the Assisted Installer’s supported static network UI for the selected release.
2. For each host, map NIC MAC addresses to approved interface names.
3. Define only interfaces, bonds, VLANs, routes, DNS, and addresses required by the architecture.
4. Ensure one intended machine-network/default-route path unless the documented design requires otherwise.
5. For bonds, validate mode, switch-side LACP/static config, VLAN tagging, MTU, and failover with the network team.
6. Check NMState configuration against current release docs and test on representative hardware where practical.
7. Apply to one host first; confirm connectivity and validation before applying the pattern to all hosts.
8. Store approved config snippets in the configuration repository; redact secrets and sensitive data.

**Static config source / NMState files:** {{REPOSITORY_PATH}}  
**MAC-to-NIC review:** {{REVIEWER / DATE}}

| Hostname | Machine NIC / MAC | Second NIC / MAC | Bond/VLAN | Static IP or DHCP | Route/DNS verified |
|---|---|---|---|---|---|
| {{HOST-01}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-02}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-03}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-04}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-05}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-06}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-07}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-08}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |
| {{HOST-09}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{VALUE}} | {{YES/NO}} |

**Image slot — NIC mapping / host networking**  
<!-- Insert: ![Host networking](./images/{{CLUSTER_NAME}}/07-host-networking.png) -->

### Step 8 — Review host and cluster validations

1. Wait for discovery and configuration validations to finish.
2. Review every host-level and cluster-level validation in the UI.
3. Resolve all blocking failures. Do not bypass one without a documented Red Hat-supported resolution.
4. Review each warning. Record impact, decision, approver, and mitigation for any warning that remains.
5. Confirm control-plane count, host roles, network reachability, DNS, NTP, registry connectivity, hardware, disk, and cluster configuration checks.
6. Re-run validations after corrections. Confirm all blocking checks pass.
7. Save a sanitized report or screenshot and record final validation time.

| Validation item | Host/cluster | Severity | Result | Resolution / exception | Owner |
|---|---|---|---|---|---|
| {{VALIDATION}} | {{HOST/CLUSTER}} | {{BLOCKING/WARNING}} | {{PASS/FAIL}} | {{DETAILS}} | {{OWNER}} |

**Blocking failures remaining:** {{0 REQUIRED}}  
**Warnings accepted with rationale:** {{NONE_OR_LINK}}  
**Final validation timestamp:** {{TIMESTAMP}}  
**Image slot — preinstallation validations**  
<!-- Insert: ![Preinstallation validation](./images/{{CLUSTER_NAME}}/08-preinstall-validation.png) -->

### Step 9 — Final review and start installation

Before selecting Install cluster (or the equivalent current UI action):

- [ ] Cluster name, base domain, version, architecture, topology, and platform are correct.
- [ ] API/ingress VIPs and DNS match the approved design.
- [ ] All hosts are identified, assigned to the approved role, and Ready.
- [ ] Installation disks have been double-checked and destructive use approved.
- [ ] Machine/pod/service networks and MTU are correct and non-overlapping.
- [ ] Optional Operators and custom manifests are approved and supported.
- [ ] All blocking validations pass; warnings have documented disposition.
- [ ] Change approver and deployment lead authorize installation.
- [ ] Summary, validation output, approvals, and images are captured without secrets.

**Final review / second reviewer:** {{NAMES}}  
**Approval timestamp:** {{TIMESTAMP}}  
**Install action / timestamp:** {{NAME / TIMESTAMP}}

**Image slot — final configuration summary / approved start**  
<!-- Insert: ![Final review](./images/{{CLUSTER_NAME}}/09-final-review.png) -->

### Step 10 — Monitor installation

1. Keep the Assisted Installer cluster page open and monitor every installation stage.
2. Record start/end times, status transitions, warnings, retries, and any host failure/reinstallation.
3. Do not reboot or power off nodes during installation unless directed by a supported recovery procedure.
4. If installation fails, save the failure message and stage/host details; open an incident/change record and follow release-specific troubleshooting.
5. Do not repeatedly restart or recreate the cluster before understanding the failure and data/disk consequences.
6. On completion, record API URL, console URL, cluster ID, and the restricted kubeconfig vault reference. Never paste kubeconfig contents here.

| Stage / event | Start | End | Result | Notes / incident |
|---|---|---|---|---|
| Discovery | {{TIME}} | {{TIME}} | {{RESULT}} | {{NOTES}} |
| Host preparation | {{TIME}} | {{TIME}} | {{RESULT}} | {{NOTES}} |
| Control-plane bootstrap | {{TIME}} | {{TIME}} | {{RESULT}} | {{NOTES}} |
| Operators / convergence | {{TIME}} | {{TIME}} | {{RESULT}} | {{NOTES}} |
| Install complete | {{TIME}} | {{TIME}} | {{RESULT}} | {{NOTES}} |

**Image slot — installation progress**  
<!-- Insert: ![Installation progress](./images/{{CLUSTER_NAME}}/10-install-progress.png) -->

**Image slot — successful installation / console overview**  
<!-- Insert: ![Installation complete](./images/{{CLUSTER_NAME}}/11-install-complete.png) -->

## 6. Post-installation verification

Use an authorized administrative account. Save sanitized output to the controlled evidence location. Record unexpected output as an incident or finding.

### 6.1 Access and platform health

1. Download kubeconfig from the approved console and store it in the secrets vault; do not commit or attach it.
2. Set the restricted kubeconfig path and verify cluster identity.
3. Check version, Operators, nodes, machine config pools, storage classes, system pods, and events.
4. Confirm nodes have expected roles, names, addresses, and Ready status.
5. Confirm critical ClusterOperators are available and no unresolved degraded/updating condition remains.
6. Check events and investigate warnings/errors relevant to API, networking, storage, certificates, and workloads.
7. Verify API and console access from approved management networks and application ingress from approved client networks.
8. Record result and exact date/time.

~~~bash
oc whoami
oc whoami --show-server
oc get clusterversion
oc get clusteroperators
oc get nodes -o wide
oc get machineconfigpools
oc get storageclass
oc get pods -A
oc get events -A --sort-by=.lastTimestamp
~~~

**Kubeconfig vault reference (no secret):** {{REFERENCE}}  
**Health output location:** {{CONTROLLED_EVIDENCE_LINK}}  
**Health result:** {{PASS/FAIL_AND_FINDINGS}}

### 6.2 API, DNS, and ingress

- [ ] API FQDN resolves to expected API VIP(s).
- [ ] API endpoint responds from approved management networks.
- [ ] Wildcard application DNS resolves to ingress VIP(s).
- [ ] Test application route works through expected load balancers and TLS trust chain.
- [ ] External load-balancer backend health and failover verified if used.
- [ ] Internal service, pod-to-pod, node-to-pod, and approved egress paths validated.
- [ ] NetworkPolicy behavior tested with allowed and denied traffic cases.

**Test tool/results:** {{DETAILS}}  
**Evidence:** {{LINK}}

### 6.3 Storage

- [ ] Expected CSI/Storage Operators are healthy.
- [ ] StorageClasses match approved names, defaults, reclaim policies, access modes, and encryption claims.
- [ ] Test PVC provisioning, pod mount, write/read, detach/reattach, and expansion if supported.
- [ ] Verify snapshots only if supported and included in the design.
- [ ] Monitoring and alerting for capacity, latency, and backend health are operational.
- [ ] Complete application data backup and restore test.

**Storage test evidence:** {{LINK}}  
**Backup/restore record:** {{LINK}}

### 6.4 Security, identity, and observability

- [ ] Approved identity login and group/role mapping work.
- [ ] Least-privilege and denied-access cases are verified.
- [ ] Break-glass access is stored, restricted, and tested per policy.
- [ ] Pod security controls and approved exceptions are reviewed.
- [ ] Audit logs are retrievable at the approved external destination.
- [ ] Platform and workload alerts reach the named response team.
- [ ] Logs are forwarded to durable external storage with required access and retention.
- [ ] Image registry, scanning, and promotion policies are operational.
- [ ] No cluster/application secrets are present in Git or deployment evidence.

**Security review:** {{LINK}}  
**Alert delivery test:** {{LINK}}  
**Log retention evidence:** {{LINK}}

### 6.5 Backup, recovery, and handover

- [ ] Release-specific etcd backup is configured outside the cluster’s primary failure domain.
- [ ] Application data/configuration backup owners, schedules, retention, and restore sequence are documented.
- [ ] RTO/RPO are approved by service owners.
- [ ] Cluster rebuild and configuration restoration procedure is versioned and reviewed.
- [ ] Node maintenance/drain behavior and workload disruption controls are understood.
- [ ] On-call, support entitlement, incident escalation, and vendor contacts are published.
- [ ] Architecture diagrams and inventory reflect deployed system.
- [ ] All MUST controls have status and evidence in the production control register.

**RTO/RPO approval:** {{LINK}}  
**Restore/failure exercise:** {{LINK}}  
**Handover acceptance:** {{LINK}}

## 7. Day-2 configuration record

Record approved post-install configuration. Link to GitOps repositories or change tickets rather than copying large manifests.

| Configuration item | Desired state / source | Applied date | Change ID | Validation / rollback reference |
|---|---|---|---|---|
| Identity provider and RBAC | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| Ingress certificates / DNS | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| Storage operators / classes | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| LSO / local PVs, if used | {{LINK_OR_N/A}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| NMState / node network policy | {{LINK_OR_N/A}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| Monitoring / alerting | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| Logging / audit forwarding | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| Backup / recovery integration | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| GitOps / configuration management | {{LINK}} | {{DATE}} | {{CHANGE}} | {{LINK}} |
| Other Operators | {{LIST_AND_COMPATIBILITY}} | {{DATE}} | {{CHANGE}} | {{LINK}} |

## 8. Deployment event log

| Date/time + timezone | Step | Operator | Activity / decision | Result | Evidence / change / incident |
|---|---|---|---|---|---|
| {{TIMESTAMP}} | {{STEP}} | {{NAME}} | {{DETAILS}} | {{RESULT}} | {{LINK}} |

## 9. Screenshot and diagram evidence index

Screenshots are supporting evidence only. Store originals in a controlled repository, protect sensitive material, and link to the approved evidence location. Do not store secrets or unrestricted customer data in Git.

| ID | Step / view | File path or evidence link | Capture date | Captured by | Redaction reviewed |
|---|---|---|---|---|---|
| IMG-01 | Installation approach / topology | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-02 | Cluster details | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-03 | Network settings | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-04 | Discovery ISO | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-05 | Discovered hosts | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-06 | Host roles and disks | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-07 | Host networking | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-08 | Preinstallation validation | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-09 | Final review | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-10 | Installation progress | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-11 | Successful installation | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-12 | Service mesh control plane / injected sidecar, if used | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| IMG-13 | Operator selection/subscriptions | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| DIA-01 | Architecture context | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| DIA-02 | Physical deployment/network | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| DIA-03 | Security/trust/data flow | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |
| DIA-04 | Resilience/recovery | {{PATH_OR_LINK}} | {{DATE}} | {{NAME}} | {{YES/NO}} |

## 10. Risks, exceptions, and deviations

| ID | Requirement / design area | Risk or deviation | Impact | Compensating control | Owner | Approver | Expiry / review |
|---|---|---|---|---|---|---|---|
| {{RISK-001}} | {{CONTROL_ID}} | {{DETAILS}} | {{IMPACT}} | {{MITIGATION}} | {{OWNER}} | {{APPROVER}} | {{DATE}} |

**Unplanned deviations during deployment:** {{NONE_OR_LIST}}  
**Follow-up actions / tickets:** {{LIST}}

## 11. Acceptance and sign-off

| Role | Name | Decision | Comments / conditions | Date |
|---|---|---|---|---|
| Business/service owner | {{NAME}} | {{APPROVE/REJECT/CONDITIONAL}} | {{COMMENTS}} | {{DATE}} |
| Platform owner | {{NAME}} | {{APPROVE/REJECT/CONDITIONAL}} | {{COMMENTS}} | {{DATE}} |
| Architecture reviewer | {{NAME}} | {{APPROVE/REJECT/CONDITIONAL}} | {{COMMENTS}} | {{DATE}} |
| Security/risk reviewer | {{NAME}} | {{APPROVE/REJECT/CONDITIONAL}} | {{COMMENTS}} | {{DATE}} |
| Operations/on-call owner | {{NAME}} | {{APPROVE/REJECT/CONDITIONAL}} | {{COMMENTS}} | {{DATE}} |

**Final acceptance:** {{APPROVED | REJECTED | CONDITIONAL}}  
**Conditions and due dates:** {{DETAILS_OR_NONE}}  
**Next architecture / control review:** {{YYYY-MM-DD}}

## 12. Troubleshooting notes

Record symptoms, evidence, diagnosis, action, and outcome. Do not paste credentials or unredacted diagnostic bundles here.

| Timestamp | Symptom / Assisted Installer stage | Evidence / error ID | Action taken | Result / follow-up |
|---|---|---|---|---|
| {{TIME}} | {{DETAILS}} | {{LINK}} | {{ACTION}} | {{RESULT}} |

For a failed installation, preserve exact error text, affected host(s), release, stage, and timestamp. Follow the supported troubleshooting procedure for that release and contact Red Hat Support when the failure is not understood.

## 13. Document completion checklist

- [ ] All placeholders replaced or explicitly marked N/A with a reason.
- [ ] Planned and as-built values are distinguished.
- [ ] Architecture and network diagrams reflect deployed cluster.
- [ ] Host, role, NIC, disk, rack, and power inventories are complete and reviewed.
- [ ] Assisted Installer steps and validations have timestamps and evidence.
- [ ] Screenshots are present, redacted, indexed, and stored in the approved location.
- [ ] Secrets, tokens, kubeconfig data, BMC credentials, and private keys are absent.
- [ ] Post-install health, security, storage, backup/restore, and alert tests are recorded.
- [ ] Production control statuses and exceptions are linked.
- [ ] Required owners signed acceptance.
- [ ] Final document is committed or archived under records policy.

## 14. References

Use documentation for the exact OCP release. Red Hat may revise UI labels and supported options; re-check references before each installation.

### 14.1 Assisted Installer

1. [Installing on-premise with Assisted Installer — OCP 4.22](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/installing_on-premise_with_assisted_installer/)
2. [Assisted Installer product guide — current web workflow](https://docs.redhat.com/en/documentation/assisted_installer_for_openshift_container_platform/2026/html-single/installing_openshift_container_platform_with_the_assisted_installer/index)
3. [About the Assisted Installer](https://docs.redhat.com/en/documentation/assisted_installer_for_openshift_container_platform/2026/html/installing_openshift_container_platform_with_the_assisted_installer/about-ai)
4. [Installing with the Assisted Installer web console](https://docs.redhat.com/en/documentation/assisted_installer_for_openshift_container_platform/2026/html/installing_openshift_container_platform_with_the_assisted_installer/installing-with-ui)
5. [Booting hosts with the discovery image](https://docs.redhat.com/en/documentation/assisted_installer_for_openshift_container_platform/2026/html/installing_openshift_container_platform_with_the_assisted_installer/assembly_booting-hosts-with-the-discovery-image)
6. [Preinstallation validations](https://docs.redhat.com/en/documentation/assisted_installer_for_openshift_container_platform/2026/html/installing_openshift_container_platform_with_the_assisted_installer/assembly_preinstallation-validations)
7. [Customizing Assisted Installer deployments using Operators and bundles](https://docs.redhat.com/en/documentation/assisted_installer_for_openshift_container_platform/2026/html/installing_openshift_container_platform_with_the_assisted_installer/customizing-with-bundles-and-operators)
8. [OpenShift Service Mesh 3.0 sidecar injection](https://docs.redhat.com/en/documentation/red_hat_openshift_service_mesh/3.0/html/installing/ossm-sidecar-injection)
9. [OpenShift Service Mesh 3.0 installation and update strategies](https://docs.redhat.com/en/documentation/red_hat_openshift_service_mesh/3.0/html/installing/ossm-installing-service-mesh)
10. [OpenShift Container Platform Operators guide](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/operators/)

### 14.2 Bare-metal and production readiness

7. [Preparing for bare-metal cluster installation](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/installing_on_bare_metal/preparing-to-install-on-bare-metal)
8. [User-provisioned infrastructure on bare metal](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/installing_on_bare_metal/user-provisioned-infrastructure)
9. [OpenShift architecture](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/architecture/)
10. [OpenShift Container Platform life cycle policy](https://access.redhat.com/support/policy/updates/openshift)
11. [Security and compliance](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/security_and_compliance/)
12. [Authentication and authorization](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/authentication_and_authorization/)
13. [Storage](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/storage/)
14. [Network security](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/network_security/)
15. [Monitoring stack for Red Hat OpenShift](https://docs.redhat.com/en/documentation/monitoring_stack_for_red_hat_openshift/4.22/)
16. [Backing up and restoring etcd](https://docs.redhat.com/en/documentation/openshift_container_platform/4.22/html/etcd/backing-up-and-restoring-etcd-data)
