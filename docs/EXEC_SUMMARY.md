[EXEC_SUMMARY.md](https://github.com/user-attachments/files/22061109/EXEC_SUMMARY.md)
# EXECUTIVE SUMMARY — ITSM + Assets + KB — GLPI + Snipe‑IT + BookStack


**What it is:** Service desk/CMDB (GLPI), asset mgmt (Snipe‑IT), and knowledge base (BookStack).  
**Replaces/reduces:** ServiceNow/Atlassian bundles for SMB/mid‑market footprints.  
**Integrates with:** hn-support bridge, g-oss-observability (SLOs), g-oss-security (SSO).


    ---

    ## Where it fits
    ```
    Users → GLPI (tickets/CMDB) ↔ Snipe‑IT (assets)
                     ↘ KB (BookStack) ↔ LLM bridge for suggestions
    ```

    ## Common oil & gas use cases
    - Tailor per asset, field, and site constraints (examples in README).

    ## Service levels (targets)
    | Metric | Target (Phase 1) | Target (Phase 2) | Notes |
    |---|---:|---:|---|
    | Availability | ≥ 99.5% | ≥ 99.9% | Business hours vs 24×7 varies by module |
    | MTTR | < 30m | < 15m | On-call runbooks + automation |
    | Throughput/Lag | Module-specific | Module-specific | e.g., MQTT msgs/s, NIDS pps |
    | Retention | 30–90d hot | 180–365d hot | Cold in Iceberg/object storage |

    ## Security & compliance
    - SSO via Keycloak, secrets in Vault, policy-as-code via OPA.
    - Network segmentation; **OT mirror is read‑only** by design.
    - TLS everywhere exposed; audit logs to OpenSearch/Loki; metrics to Prometheus.

    ## Rollout plan (risk-controlled)
    1) **Pilot 0–90d:** narrow scope; define acceptance SLOs; instrument dashboards.
    2) **Parallel 90–180d:** run alongside existing tools; compare counts/latency.
    3) **Cutover 180–365d:** switch consumers; keep rollback window; train owners.

    ## KPIs for leadership
    - Value: license $ removed, $/endpoint or $/GB, time‑to‑provision.
    - Reliability: freshness/lag, success rate, MTTR.
    - Security: % behind SSO, secrets in Vault, policy violations blocked.

    ## Risks & mitigations
    - Skills gaps → training/pairing/runbooks.
    - Data volume bursts → capacity planning + autoscaling.
    - Access scope creep → RBAC + network policy; audit.

    ## Proprietary → OSS mapping (examples)
    - Listed in the module README; adjust to your environment.

    **Maintainers:** @genioCE/maintainers • **Contact:** brian@hewesguyen.com
