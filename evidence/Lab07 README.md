Highest Architectural Risk
Simultaneous rental requests for the same item may result in conflicting reservations and incorrect availability counts. This would violate FR-07, FR-11 and the duplicate-booking acceptance criterion.

Mitigation
Lock reservation variable incase of concurrent access.
Reserve quantities immediately after owner acceptance.
Release reservations after cancellation or timeout.
Enforce availability validation before activation.

The architecture decision will be reconsidered if:
Duplicate bookings occur during testing.
Reservation logic fails under concurrent access.
Quantity values become inconsistent during testing.
