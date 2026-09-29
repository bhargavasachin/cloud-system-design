# System Design: Release Safety and Rollback

## 1. Problem

A deployment pipeline can successfully deliver a new version while the application is still unhealthy. A safer release path needs to connect deployment progress with health signals and provide a controlled rollback path.

## 2. Requirements

- Release an immutable application artifact.
- Deploy through an auditable desired-state mechanism.
- Detect rollout failures early.
- Use application and platform health signals during promotion.
- Prevent an unhealthy release from being promoted further.
- Make rollback explicit and repeatable.
- Preserve enough evidence to diagnose the decision.

## 3. High-level design

```text
Developer
   |
   v
Source Control
   |
   v
CI: build + test
   |
   v
Immutable Artifact
   |
   v
Deployment Controller
   |
   +--------------------+
   |                    |
   v                    v
Rollout Health       Application Health
   |                    |
   +---------+----------+
             |
             v
       Release Decision
        /           \
       /             \
    Promote        Stop/Rollback
       |               |
       v               v
 Next Environment   Known-good version
```

## 4. Health signals

Signals should be selected for the failure modes that matter to the application. Typical inputs include:

- desired vs available replicas
- readiness state
- HTTP error rate
- latency
- restart rate
- dependency health
- recent deployment events

A signal should have an explicit threshold and a clear action. Avoid collecting metrics simply because they are available.

## 5. Decision flow

1. CI produces an immutable artifact.
2. The artifact is deployed to the target environment.
3. The deployment controller reports rollout progress.
4. Health checks evaluate the application and platform.
5. If the checks pass, promotion can continue.
6. If a required check fails, promotion stops.
7. If the release is already active and recovery is required, desired state is changed back to the last known-good version.
8. The rollback is reconciled and verified using the same health checks.

## 6. Failure modes

### New Pods never become Ready

Inspect scheduling, image availability, configuration, probes and dependency access. Do not treat a readiness failure as proof that the application process must be restarted.

### Application is Ready but error rate increases

A basic Kubernetes readiness check may remain green while the application is functionally unhealthy. Application-level signals are needed for this class of failure.

### Monitoring is unavailable

The release controller should fail closed for checks that are mandatory for the safety decision. It should not interpret missing telemetry as healthy telemetry.

## 7. Operational considerations

The release decision should be observable: record the artifact version, environment, health measurements, decision, and rollback target. Human approval can be retained for higher-risk production changes.

The design intentionally separates **detection** from **remediation**. An automated system can identify that a release is unhealthy while keeping destructive or high-impact actions behind an explicit policy or approval boundary.

## 8. Trade-offs

A fully automatic rollback can reduce recovery time but can also hide application-level problems when the health signal is incomplete. A human approval step adds latency but can be appropriate for high-impact production changes. The right boundary depends on the reliability of the signals and the consequences of an incorrect action.
