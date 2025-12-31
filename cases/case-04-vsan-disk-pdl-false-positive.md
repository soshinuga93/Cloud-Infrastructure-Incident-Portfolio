# Case 04 – vSAN Disk Failure False Positive: PDL / Faulty NAA Cleared After Reboot

## Environment
- Platform: Dell VxRail with vSAN
- Component: vSAN disks (cache/capacity) monitoring
- Alert Type: Disk failure / predictive disk alarm
- Context: Customer concerned about dispatch/part replacement

## Issue Summary
Disk alarm was reported on a host:
- Physical view appeared healthy
- Cluster disk management indicated failure for a specific disk (by NAA)
Command output was inconsistent, suggesting a possible false positive.

## Symptoms
- Alarm indicating disk failure in cluster UI
- Physical hardware view showing disk as OK
- Conflicting CLI results:
  - `vdq -qH` showed no PDL in output
  - `esxcli vsan storage list` flagged the device as faulty
- iDRAC showed warning alert

## Investigation & Analysis
- Collected diagnostics:
  - TSR logs from environment
- Performed host-side checks:
  - `vdq -qH`
  - `esxcli vsan storage list`
- Noted inconsistency between disk health sources
- Considered uptime factor (host had long uptime)
- Chose a non-invasive validation approach before replacement

## Root Cause
Most likely a **transient controller/telemetry state** or stale disk health condition after long uptime, producing a false positive disk fault.

## Resolution
- Power cycled the affected host to refresh hardware state
- Re-validated disk health post-reboot
- Error cleared after host power cycle

## Outcome
- Disk alarm resolved
- Field engineer dispatch placed on hold
- Customer retained part while monitoring for recurrence

## Follow-up / Monitoring Plan
- Monitor environment until agreed date/time window
- If alarm re-occurs, proceed with planned replacement workflow

## Key Skills Demonstrated
- vSAN disk triage methodology
- Evidence-based decision-making (avoid unnecessary dispatch)
- Log collection and cross-validation (UI vs CLI vs iDRAC)
- Clear customer-facing plan of action
