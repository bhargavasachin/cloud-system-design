# 02 — Health-aware deployment control

## Problem

A deployment should not be considered successful only because Kubernetes accepted a new ReplicaSet. The release process needs to distinguish **submitted**, **available**, and **healthy enough to promote**.

## Requirements

- Deploy a specific immutable application version.
- Observe rollout progress.
- Evaluate application and platform health signals.
- Stop promotion when health signals cross a defined threshold.
- Preserve enough information to identify the deployed version and recover.
- Keep a human approval point for higher-risk recovery actions.

## Control flow

```text
Release Request
      |
      v
Validate version/config
      |
      v
Deploy to target
      |
      v
Observe rollout
      |
      +---- unhealthy ----> Stop promotion
      |                         |
      |                         v
      |                   Capture evidence
      |                         |
      |                         v
      |                  Human decision
      |
      +---- healthy ------> Promote
```

## Health signals

Use multiple signals instead of a single probe: rollout status, readiness, error rate, request latency, restart count and relevant platform alarms. The exact thresholds should be environment-specific and reviewed with the service owner.

## Failure handling

If the rollout stalls, stop the next promotion and retain the evidence from the failed version. Recovery should be an explicit operation. A rollback can restore the last known-good application version, but it should not silently erase evidence needed for diagnosis.

## Why this pattern matters

The deployment controller owns desired state; the release control layer owns promotion policy. Keeping those concerns separate makes the workflow easier to test and prevents a pipeline from treating "kubectl apply succeeded" as equivalent to "the release is healthy."

## Operational safety

Automated actions should have clear boundaries. A system that can recommend or prepare a recovery action does not automatically need authority to perform every destructive operation. High-impact changes should remain reviewable and auditable.
