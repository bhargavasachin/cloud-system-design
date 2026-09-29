# Design guide

These designs use the same review structure so the important engineering decisions are easy to compare.

## 1. Problem and scope

State what is being built, who uses it, and what is deliberately out of scope.

## 2. Requirements

Separate functional requirements from operational requirements such as availability, latency, recovery, security and auditability.

## 3. Constraints and assumptions

Call out assumptions before choosing technologies. This avoids architecture by habit.

## 4. Capacity and data flow

Estimate the important scale dimensions and describe the path of a normal request or event.

## 5. Architecture

Show the major components and their ownership boundaries. Keep implementation detail below the level needed to explain the design decision.

## 6. Failure modes

For each important dependency, ask what happens when it is unavailable, slow, inconsistent or partially successful.

## 7. Observability

Define the signals that distinguish a healthy system from a system that is merely running.

## 8. Security and authorization

Identify trust boundaries, sensitive data and actions that require explicit authorization.

## 9. Scaling and recovery

Explain what scales independently, where bottlenecks appear, and how the system returns to a known-good state after failure.

## 10. Trade-offs

Record what was chosen, what was rejected, and why. A good design is a set of deliberate trade-offs rather than a list of technologies.
