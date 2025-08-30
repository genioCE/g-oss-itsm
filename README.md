# ITSM + Assets + KB — GLPI + Snipe‑IT + BookStack


**What it is:** Service desk/CMDB (GLPI), asset mgmt (Snipe‑IT), and knowledge base (BookStack).  
**Replaces/reduces:** ServiceNow/Atlassian bundles for SMB/mid‑market footprints.  
**Integrates with:** hn-support bridge, g-oss-observability (SLOs), g-oss-security (SSO).


    ## Components
    - See `docker-compose.yml` for images and versions.
    - Helm chart skeleton is under `charts/g-oss-itsm` (namespace defaults to `platform`).

    ## Quick start (POC)
    ```bash
    cp .env.example .env   # if provided
    docker compose up -d
    ```

    ## Default ports (POC)
    - Refer to `docker-compose.yml` for host port bindings.

    ## Architecture (at a glance)
    ```
    Users → GLPI (tickets/CMDB) ↔ Snipe‑IT (assets)
                     ↘ KB (BookStack) ↔ LLM bridge for suggestions
    ```

    ## O&G use cases
    - Document in your repo issues; examples:
      - EDR/SIEM: tier‑1 alerting, endpoint posture, Windows event collection.
      - Fleet: inventory snapshot, vulnerable software query, CIS checks.
      - NIDS: vendor tunnel monitoring, remote site SPAN analysis.
      - Backup/DR: daily S3 snapshots; quarterly restore tests.
      - DevOps: private git/CI for regulated repos; image promotion.
      - ITSM: ticket→KB pipeline; asset lifecycle control.
      - Comms: on‑prem chat for field ops when SaaS is restricted.
      - OT mirror: analytics on tags without control‑path access.
      - IoT broker: device fleets with Sparkplug and ACLs.
      - Edge flow: normalize vendor payloads to JSON schemas.
      - TSDB: rate/pressure dashboards; alerting on anomalies.
      - GIS: pad/lease maps; field routing overlays.

    ## Configuration
    - Edit environment variables in `.env` and the compose file.
    - For Helm, set values in `charts/g-oss-itsm/values.yaml`.
    - SSO via Keycloak: configure OIDC in proxy and app where supported.
    - Secrets: reference Vault injectors or template into env at deploy time.

    ## Production hardening checklist
    - [ ] TLS at the edge (reverse proxy: Caddy/Traefik/Nginx)
    - [ ] **SSO via Keycloak**, roles mapped to app RBAC
    - [ ] Secrets in **Vault**, not in `.env`
    - [ ] Observability: Prometheus exporters + Grafana dashboards
    - [ ] Logs shipped to Loki/OpenSearch with retention
    - [ ] Backups (if stateful): restic/pgBackRest/S3 lifecycle
    - [ ] Network policies + firewall rules (least privilege)
    - [ ] Image signing/SBOM and vulnerability scans in CI

    ## Sizing & performance
    - Start small (1–2 vCPU, 2–8 GB RAM per service) and profile.
    - Scale stateful components first (indexers/brokers/TSDB).

    ## Integration points
    - **Security:** g-oss-security (SSO/Vault/OPA)
    - **Observability:** g-oss-observability (metrics/logs/traces)
    - **Data platform:** g-oss-core (lakehouse) and others as noted

    ## Backup & DR
    - Use `g-oss-backup-dr` patterns (restic → object storage).
    - Document RPO/RTO per service and test restores quarterly.

    ## Proprietary → OSS mapping (examples)
    - EDR/SIEM: Splunk ES → Wazuh/OpenSearch
    - Fleet: endpoint analytics → FleetDM/osquery
    - NIDS: Commercial IDS → Zeek/Suricata
    - Backup: enterprise backup → restic/pgBackRest/Velero
    - DevOps: GitHub Enterprise/Atlassian/paid registries → Gitea/Drone/Harbor
    - ITSM: ServiceNow/Atlassian → GLPI/Snipe‑IT/BookStack
    - Comms: Slack/Teams add‑ons → Mattermost/Matrix
    - OT: proprietary historian add‑ons → OPC UA mirror + MQTT + TSDB
    - IoT broker: enterprise brokers → EMQX CE
    - Edge ETL: commercial edge pipelines → Node‑RED/NiFi
    - TSDB: Splunk Observability/Influx Enterprise → VictoriaMetrics/QuestDB
    - GIS: ESRI bundles (subset) → PostGIS/GeoServer

    ## Roadmap ideas
    - CI: add lint/tests (compose config, Helm lint)
    - Helm: publish charts to GHCR (OCI), pin image digests
    - Azure: CNAB packaging for marketplace (AKS extension)

    **Maintainers:** @genioCE/maintainers • **Contact:** brian@hewesguyen.com
