# Case 08 – ESXi Host Disconnected From vCenter: Host Stuck During Reboot, Recovered via Console Power Cycle

## Environment
- Platform: VxRail / ESXi host managed by vCenter
- Access: Host virtual console (out-of-band / remote console)
- Condition: Host reachable by ping, but management plane unresponsive

## Issue Summary
Customer reported a node disconnected from vCenter:
- Host pingable
- DCUI login timed out
- Management network couldn’t ping gateway/DNS
- Restarting management network and agents didn’t resolve it
VMs appeared disconnected in vCenter but were still reachable.

## Symptoms
- Host disconnected in vCenter
- DCUI timeout
- Management network connectivity issues (gateway/DNS unreachable)
- Host appeared stuck during reboot in console
- VMs “disconnected” but operating (reachable)

## Investigation & Analysis
- Considered common triggers:
  - pending firmware updates
  - resync behavior
  - environmental/power events
- Verified through console:
  - host was hung/stuck during reboot
- Confirmed workload continuity:
  - VMs reachable even while shown disconnected

## Root Cause
Host was stuck in an incomplete reboot / hung state, leaving the management plane unresponsive while workloads remained running.

## Resolution
- Used virtual console to:
  - power off the host
  - power the host back on (clean boot)
- Host completed boot fully
- Host reconnected to vCenter
- VMs began migrating back normally / rebalancing

## Outcome
- Host connectivity restored
- Customer satisfied with recovery
- Customer agreed to collect and send logs for post-incident review

## Key Skills Demonstrated
- ESXi host recovery under partial management-plane failure
- Risk-aware actions (validated VM reachability)
- Fast restoration of vCenter connectivity
- Post-incident logging and analysis mindset
