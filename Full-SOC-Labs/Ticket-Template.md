# Ticket

Title: [Alert / Incident Name]

Severity: Low / Medium / High / Critical

Source: [Source IP / Host]

Destination: [Destination IP / Host]

Detection: [Security Onion / Suricata / Zeek / Elastic]

Triage:
[Initial assessment: what triggered the alert and whether it appears suspicious, benign, or expected.]

Analysis:
[Logs, alerts, PCAP, endpoint data, and other evidence reviewed.]

Conclusion:
[What you determined and why.]

Escalation:
[None / Escalated to Tier 2 / Escalated to Incident Response / Reason for escalation.]

Action:
[Investigated, contained, blocked, monitored, escalated, or no action required.]

Status: Open / Investigating / Escalated / Resolved / False Positive

Evidence:
[PCAP, alert ID, screenshots, logs, queries, etc.]


1. ALERT
   ↓
2. TRIAGE
   "Is this worth investigating?"
   ↓
3. ANALYSIS
   "What actually happened?"
   ↓
4. CONCLUSION
   "Is it malicious, benign, or authorized?"
   ↓
5. ESCALATION
   "Does someone else need to handle this?"
   ↓
6. ACTION
   "What did we do about it?"
   ↓
7. STATUS
   "Where does the incident stand?"
   ↓
8. EVIDENCE
   "Can another analyst reproduce/verify my findings?"
