# Case 05 – vCenter GUI Down “No Healthy Upstream”: Machine SSL Certificate Expired

## Environment
- Platform: VxRail environment with vCenter
- Access: SSH to vCenter (putty)
- Incident Trigger: vCenter VM reboot

## Issue Summary
After customer rebooted vCenter VM, the vCenter GUI became inaccessible and showed:
**“No Healthy Upstream”**
Investigation confirmed the **vCenter Machine SSL certificate was expired**.

## Symptoms
- vCenter GUI unreachable with upstream health error
- Customer initially rebooted vCenter because login was failing
- Logs showed authentication/session errors

## Investigation & Analysis
- Verified certificate status via SSH
  - Confirmed Machine SSL certificate expired
- Reviewed key log file for service/auth errors:
  - `/var/log/vmware/vpxd-svcs/vpxd-svcs.log`
- Determined certificate renewal required

## Root Cause
Expired **Machine SSL certificate** caused vCenter services/UI components to fail health checks, resulting in “No Healthy Upstream.”

## Resolution
- Took both online and offline snapshots of:
  - VxRail Manager
  - vCenter
- Followed documented renewal procedure for Machine SSL certificate (vCenter certificate tooling)
- Completed certificate renewal successfully
- Confirmed vCenter GUI restored
- Reset certificate-related alarms to green

## Outcome
- vCenter GUI access fully restored
- Cluster alarms cleared
- Customer confirmed normal access and operations

## Key Skills Demonstrated
- vCenter certificate troubleshooting
- Safe change management (snapshots before cert operations)
- Log analysis and service health validation
- Customer communication and closure readiness
