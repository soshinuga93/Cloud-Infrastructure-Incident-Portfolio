#  Case 02 – VxRail Upgrade Extraction Stuck: Lock Held by BACKUP

## Environment
- Platform: Dell VxRail
- vCenter: External vCenter
- Upgrade Type: Self-upgrade via VxRail Manager (LCM)
- Stage: Bundle extraction (~50%)

## Issue Summary
Customer’s VxRail upgrade was stuck during **extraction at 50%**. Logs showed the upgrade workflow was blocked by an internal **lock held by BACKUP**.

## Symptoms
- Upgrade extraction stalled at ~50% and would not progress
- Retrying showed similar behavior

## Investigation & Analysis
- Reviewed LCM logs and found lock conflict:
  - HTTP 409 CONFLICT
  - `Lock conflicts due to the lock is acquired by other operation`
  - `locked_by: BACKUP`
- Confirmed no other visible upgrade operation running in the UI

## Root Cause
The upgrade process was blocked because a **backup-related process** held an exclusive lock on the upgrade workflow.

## Resolution
- Released/unlocked the upgrade lock held by BACKUP
- Enabled Advanced Mode on VxRail Manager (to support required operations)
- Restarted/bounced VxRail Manager services to clear stale state
- Restarted extraction

## Outcome
- Extraction completed successfully
- Pre-check (vxverify) was clean
- Customer continued monitoring and proceeded with upgrade workflow

## Key Skills Demonstrated
- Log-driven troubleshooting (LCM)
- Understanding of VxRail locking mechanisms
- Safe recovery actions (unlock + service restart)
- Customer guidance for self-upgrade workflows
