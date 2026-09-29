# Cloud System Design

Architecture notes for cloud/platform systems, written with an interview-friendly structure: requirements, constraints, high-level design, data/control flow, failure modes, observability and trade-offs.

The goal is not to memorize diagrams. It is to show the reasoning used to turn an operational problem into a system that can be deployed and supported.

## Design template

Each design follows the same sequence:

1. **Requirements** — functional and non-functional requirements.
2. **Constraints** — scale, latency, availability, security and operational boundaries.
3. **High-level architecture** — major components and their responsibilities.
4. **Data/control flow** — what happens on the normal path.
5. **Failure paths** — what happens when dependencies or infrastructure fail.
6. **Observability** — signals needed to detect and diagnose problems.
7. **Capacity and scaling** — where bottlenecks appear and how the system responds.
8. **Trade-offs** — why one approach is selected over reasonable alternatives.

## Designs

- [01 — Smart Parking](designs/01-smart-parking.md)
- [02 — Health-aware deployment control](designs/02-health-aware-deployment-control.md)

More designs will be added using the same format rather than as disconnected architecture diagrams.

> **Portfolio note:** The designs are public engineering case studies. They are not reproductions of proprietary company architectures.
