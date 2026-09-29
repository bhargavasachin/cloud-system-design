# 01 — Smart Parking

## 1. Requirements

Drivers should be able to find available parking, reserve a space where supported, enter/exit a facility, and receive an accurate parking status.

Operators need occupancy visibility and the ability to manage lots, zones and equipment.

## 2. Core entities

- Parking lot
- Zone
- Space
- Vehicle
- Reservation
- Entry/exit event
- Occupancy state

## 3. High-level design

```text
Mobile/Web Client
       |
       v
API Gateway
       |
       +------ Reservation Service ------ Database
       |
       +------ Availability Service ----- Cache
       |
       +------ Event Ingestion ---------- Event Stream
                                             |
                                             v
                                      Occupancy Processor
                                             |
                                             v
                                      Availability Store
```

Sensors and gates produce events rather than forcing the client to poll individual devices.

## 4. Availability path

A read request checks a fast availability representation. Reservation writes use a transactional path so two clients cannot successfully reserve the same space. Occupancy events update the availability representation asynchronously.

## 5. Failure handling

If sensors stop reporting, the system should expose the age of the last known state rather than presenting stale data as current. If the cache is unavailable, the service can fall back to the authoritative store at reduced performance.

Reservation creation should be idempotent so client retries do not create duplicate reservations.

## 6. Scaling

Read-heavy availability traffic can scale independently from reservation writes. Event processing can partition by lot or zone so one noisy facility does not block unrelated facilities.

## 7. Observability

Track API latency, reservation conflicts, event-processing lag, stale occupancy age, sensor heartbeat failures and database errors.

## 8. Trade-offs

Strong consistency is more important for reservation allocation than for a momentary availability display. Separating those paths keeps the user experience responsive without weakening reservation correctness.
