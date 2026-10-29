# PsExecHunt — Investigation Report

> **Status: Report template.** Fill this in using your actual TryHackMe lab observations. Do not publish answers, flags, or restricted room material if the platform rules prohibit it.

## Objective
Investigate activity associated with PsExec or related remote-execution behavior, correlate relevant telemetry, and explain whether the evidence supports the hypothesis.

## Environment
- **Platform / room:** TryHackMe — PsExecHunt
- **Date completed:** [YYYY-MM-DD]
- **Data sources examined:** [List the actual logs, event channels, or files]
- **Tools used:** [List only tools actually used]

## Investigation questions
1. Which account initiated the activity?
2. Which host or hosts were involved?
3. What process, service, or command-line evidence was observed?
4. What is the event timeline?
5. Which evidence supports or weakens the remote-execution hypothesis?

## Evidence table

| Timestamp (with timezone) | Host | Source / Event ID | Observation | Interpretation |
|---|---|---|---|---|
| [Fill in] | [Fill in] | [Fill in] | [Sanitized observation] | [Evidence-based interpretation] |

## Investigation workflow
1. Establish the time range and affected host(s).
2. Search available telemetry for process creation, service installation, authentication, and network activity where present.
3. Correlate timestamps, usernames, process names, command lines, and source/destination hosts.
4. Separate direct observations from assumptions.
5. Build a concise timeline and state the conclusion with confidence and limitations.

## SPL scratchpad
Use only if the lab data is indexed in your Splunk instance. Adapt the index and field names to your actual data.

```spl
index=<YOUR_INDEX> earliest=<START_TIME> latest=<END_TIME>
| search <RELEVANT_HOST_OR_PROCESS>
| table _time host user EventCode Image CommandLine ParentImage
| sort 0 _time
```

This is a generic starting point, not a verified query for the room's dataset.

## Conclusion
- **Hypothesis:** [What behavior was being investigated?]
- **Supporting evidence:** [Cite observations from your evidence table]
- **Alternative explanations:** [What else could explain the activity?]
- **Conclusion and confidence:** [State only what the evidence supports]
- **Lessons learned:** [What would you investigate next in a real SOC?]

## Publication checklist
- [ ] Remove flags, passwords, tokens, personal data, and restricted lab content.
- [ ] Sanitize hostnames, usernames, and IP addresses where appropriate.
- [ ] Include your own screenshots or query output, if allowed.
- [ ] Verify every event ID, timestamp, field, and conclusion against your actual results.
