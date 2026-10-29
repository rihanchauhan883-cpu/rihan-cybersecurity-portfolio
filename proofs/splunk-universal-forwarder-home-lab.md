# Splunk Universal Forwarder — Windows-to-Splunk Home Lab

> **Status: Evidence template — complete the fields and attach sanitized screenshots before presenting this as a finished case study.**  
> The portfolio owner reported that Windows events were arriving in Splunk after configuring the Universal Forwarder. This file is a documentation scaffold, not independent verification.

## Objective
Configure a Windows endpoint to forward event data to a Splunk instance and validate that the data is searchable.

## Lab components
- **Endpoint:** Windows VM with Splunk Universal Forwarder
- **SIEM:** Splunk Enterprise on an Ubuntu VM
- **Forwarding port:** `9997` (confirm from the actual configuration)
- **Splunk Web:** `8000` (management interface; not the forwarding port)

Do not publish real credentials, private addresses, hostnames, tokens, or other sensitive configuration values.

## Procedure
1. Confirm the Splunk receiver is enabled on the SIEM.
2. Configure the Universal Forwarder deployment/receiving target.
3. Select the intended Windows event logs in the forwarder's inputs configuration.
4. Restart the forwarder if configuration changes require it.
5. Search Splunk for the endpoint's incoming events.
6. Validate timestamps, host, source, sourcetype, and event content.

## Evidence to attach
- [ ] Screenshot of the Splunk receiving configuration (sanitize addresses if needed).
- [ ] Screenshot of the forwarder's configured target and selected inputs (hide sensitive values).
- [ ] Screenshot of a Splunk search returning Windows events.
- [ ] Example SPL query used for validation.
- [ ] Date tested and versions of Splunk/Universal Forwarder.

## Validation query starter
Adjust index and sourcetype to match the actual data:
```spl
index=* host="<SANITIZED_HOST>"
| stats count by sourcetype source
| sort - count
```

## Findings
- **What arrived:** [Fill in the event sources and event IDs actually observed]
- **How ingestion was validated:** [Describe the query and result]
- **Issue encountered and resolution:** [Document only what happened]
- **Limitations / next step:** [For example, validate Sysmon or PowerShell logging separately]

## Security note
Use synthetic or sanitized values in this public write-up. Never publish passwords, tokens, raw sensitive logs, private IPs, or restricted challenge data.
