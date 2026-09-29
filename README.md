# Cloud System Design

Architecture notes for cloud/platform systems, written with an interview-friendly structure: requirements, constraints, high-level design, data/control flow, failure modes, observability and trade-offs.

The goal is not to memorize diagrams. It is to show the reasoning used to turn an operational problem into a system that can be deployed and supported.

## Design template

Each design follows the structure in [DESIGN_GUIDE.md](DESIGN_GUIDE.md):

1. **Problem and scope** — what is being built and what is out of scope.
2. **Requirements** — functional and operational requirements.
3. **Constraints and assumptions** — called out before choosing technologies.
4. **Capacity and data flow** — scale dimensions and the normal path.
5. **Architecture** — major components and ownership boundaries.
6. **Failure modes** — what happens when dependencies fail.
7. **Observability** — signals that distinguish healthy from merely running.
8. **Security and authorization** — trust boundaries and sensitive actions.
9. **Scaling and recovery** — independent scaling and return to known-good.
10. **Trade-offs** — what was chosen, what was rejected, and why.

## Designs

- [01 — Smart Parking](designs/01-smart-parking.md)
- [02 — Health-aware deployment control](designs/02-health-aware-deployment-control.md)
- [03 — Release safety and rollback](designs/03-release-safety-and-rollback.md)

More designs will be added using the same format rather than as disconnected architecture diagrams.

> **Portfolio note:** The designs are public engineering case studies. They are not reproductions of proprietary company architectures.
