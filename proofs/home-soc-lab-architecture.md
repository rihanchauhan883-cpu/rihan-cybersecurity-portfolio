# Home SOC Lab — Architecture and Telemetry Workflow

> **Status: Lab overview based on the portfolio owner's described setup.** Verify current addresses, services, and data flows before publishing screenshots or a final diagram.

## Purpose
Build a small defensive monitoring environment to practice endpoint telemetry collection, centralized log analysis, and investigation workflows.

## Components
- **Ubuntu VM:** Splunk Enterprise instance.
- **Windows endpoint VM:** Windows telemetry and Splunk Universal Forwarder.
- **Windows Server VM:** Planned/available Windows domain and server telemetry, depending on current lab state.
- **Wazuh:** Separate or additional monitoring component; document only services that are currently installed and running.

## Intended data flow
```text
Windows endpoint logs
        |
        v
Splunk Universal Forwarder
        |
        | TCP 9997 (verify receiver configuration)
        v
Splunk on Ubuntu
        |
        v
SPL searches -> dashboards / alerts -> investigation notes
```

If Wazuh is enabled, document its agent, manager, indexer, and dashboard paths separately. Do not imply Splunk and Wazuh are integrated unless that integration has been configured and tested.

## Validation checklist
- [ ] Confirm the SIEM VM is running and Splunk is healthy.
- [ ] Confirm the receiving port is listening.
- [ ] Confirm the Windows forwarder service is running.
- [ ] Confirm selected Windows event logs are enabled for collection.
- [ ] Run a Splunk search and verify recent events arrive.
- [ ] Trigger a safe, benign test event and verify it appears.
- [ ] Record the timestamp, source, sourcetype, host, and validation query.
- [ ] Capture a sanitized architecture diagram and evidence screenshots.

## Evidence log

| Date | Component tested | Test | Observed result | Evidence link |
|---|---|---|---|---|
| [YYYY-MM-DD] | [Component] | [Test] | [Actual result] | [Screenshot / commit / report] |

## What this demonstrates
When supported by completed validation evidence, this project can demonstrate familiarity with log collection, SIEM ingestion, endpoint telemetry, troubleshooting, and evidence-based investigation.

## Safety
Do not publish credentials, tokens, sensitive logs, private IPs, or unredacted configurations. Use sanitized examples in the public repository.
