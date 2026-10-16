# Development Roadmap

## Target

A roughly 12-week prototype and supervised-pilot programme, assuming a small team and access to sports-science and clinical expertise. This is not a promise of clinical validation within 12 weeks.

## Phase 1 — Foundation and capture (Weeks 1–4)

- Confirm sprint and rehabilitation workflows with practitioners.
- Define the metric dictionary and capture protocol.
- Implement athlete profiles, consent, sessions, and role permissions.
- Implement video upload, metadata validation, and quality feedback.
- Evaluate pose-estimation and multi-view biomechanics options.
- Establish storage, security, and audit foundations.

**Exit criteria:** an authorised user can create an athlete and session, upload video, and review capture quality.

## Phase 2 — Twin and analytics (Weeks 5–7)

- Implement versioned analysis jobs.
- Extract landmarks and store derived artefacts.
- Implement initial stride events and selected metrics.
- Ingest sprint split and timing data.
- Build athlete baselines and comparable-session comparisons.
- Build a basic Three.js replay and movement viewer.

**Exit criteria:** metrics have units, provenance, pipeline version, and quality status; unsupported metrics are not presented as facts.

## Phase 3 — Interpretation and rehabilitation (Weeks 8–9)

- Implement structured interpretations and evidence references.
- Add performance goals, readiness context, and intervention records.
- Add injury episodes, clinician-defined criteria, and review workflows.
- Add decision history and reports.
- Add safeguards for clearance and unsupported claims.

**Exit criteria:** insights are traceable to evidence, and clinical clearance requires authorised human sign-off.

## Phase 4 — Supervised pilot (Weeks 10–12)

- Evaluate repeatability across repeated recordings.
- Compare selected metrics with appropriate reference measurements.
- Test variation across phones, camera positions, lighting, and athletes.
- Gather coach and clinician feedback.
- Document measurement failures and model limitations.
- Prioritise the next release based on evidence.

**Exit criteria:** a pilot report describes what works, what remains unreliable, and what validation is still required.

## Post-MVP

- Expand sport-specific task modules.
- Add optional wearable and timing-gate integrations.
- Improve 3D reconstruction and individual anatomical modelling.
- Evaluate longitudinal outcome prediction only after suitable labelled data exists.
- Conduct prospective evaluations of clinical and performance usefulness.
- Consider organisation-level analytics with strong privacy protections.

## Suggested engineering sequence

1. Repository standards, threat model, and architecture decisions
2. Database schema and API contracts
3. Authentication, roles, consent, and athlete CRUD
4. Session and video ingestion
5. Background processing and quality gates
6. Biomechanics adapter interface
7. Metrics and baseline service
8. 3D viewer and comparisons
9. Evidence-linked interpretation service
10. Rehabilitation workflow and audit trail
11. Tests, observability, backup/restore, and pilot documentation
