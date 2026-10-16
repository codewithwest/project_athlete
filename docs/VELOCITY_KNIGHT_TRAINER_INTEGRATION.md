# Velocity Knight Trainer Integration

## Purpose

Integrate TwinStride AI with the existing Velocity Knight Trainer (VKT) platform instead of rebuilding its athlete-programme and coaching workflows. This document is based on the connected repositories' current source and documentation; verify details against deployed configuration before implementation.

## Repositories reviewed

- [Velocity Knight Trainer Go backend](https://github.com/codewithwest/project_velocity-knight-trainer-go) — Go GraphQL API, MongoDB-backed domain modules, session/authentication and access-control logic.
- [Velocity Knight Trainer Web frontend](https://github.com/codewithwest/project_velocity-knight-trainer-web) — Next.js frontend using Apollo Client and GraphQL.
- [TwinStride AI](https://github.com/codewithwest/project_athlete) — biomechanics, performance analysis and rehabilitation decision-support product blueprint.

## Ownership boundaries

| Capability | System of record | TwinStride responsibility |
|---|---|---|
| Athlete identity and VKT account/session | VKT | Use a stable VKT athlete identity; do not create a parallel login |
| Training programmes and exercises | VKT | Link analysis to programme/session IDs; do not duplicate programme editing |
| Training cycles and day assignments | VKT | Display context and relate assessments to a cycle/day/programme |
| Athlete feedback and coach comments | VKT | Attach evidence-backed analysis summaries as a separate, clearly attributed result or link |
| Coach/athlete sharing and permissions | VKT | Respect VKT authorization; apply additional consent and access checks for video/health data |
| Competition records | VKT | Correlate results with supported performance measurements without silently overwriting source results |
| Video, capture metadata, pose landmarks, biomechanics artefacts | TwinStride | Store and process these in TwinStride-controlled services |
| Derived metrics, uncertainty, pipeline provenance, review decisions | TwinStride | Version, audit and explain the outputs |
| Rehabilitation decisions and return-to-sport sign-off | Qualified human professional | Provide supporting evidence only; never auto-clear an athlete |

## Verified technical observations

### Web frontend

The VKT web repository is a Next.js application. Its Apollo client reads the GraphQL endpoint from `NEXT_PUBLIC_GRAPHQL_URL` and configures an authorization header plus user identity/type/token headers in `src/lib/apollo.ts`. The web app has TypeScript domain types for cycles and feedback, including day assignments, session dates, feedback entries, comments, and coach-sharing indicators.

### Go backend

The Go repository documents a GraphQL endpoint and includes domain operations for programmes, feedback, cycles, competition/stadium data, and authentication/session handling. Its README describes programme queries/mutations such as `getProgram`, `getPrograms`, `createProgram`, `updateProgram`; feedback operations such as `getFeedbacks`, `createFeedBack`, `addCommentToFeedback`; and access-control operations. Training-cycle implementation documentation and Go cycle types include `day_assignments`, `is_active`, season ordering and athlete-linked cycle information.

The backend README describes MongoDB and Firebase Google sign-in. Treat the live GraphQL schema and deployed configuration as authoritative over examples in documentation.

## Recommended integration shape

1. **Keep the existing VKT web app as the athlete/coaching experience initially.** Add a TwinStride analysis route/page or a linked module rather than replacing VKT dashboards.
2. **Use a TwinStride service boundary.** TwinStride owns upload validation, consent for video processing, asynchronous analysis jobs, biomechanics artefacts, metric provenance and reports.
3. **Associate records with VKT identifiers.** Store VKT athlete ID and, when available, programme ID, cycle ID, session ID, and competition/result ID as external references. Do not assume numeric IDs are globally unique; scope them by VKT environment/tenant and record the source.
4. **Integrate via authenticated server-to-server API.** Prefer a documented, least-privilege integration credential or token exchange. Do not expose backend secrets in Next.js public environment variables. Avoid browser-to-browser trust based only on a client-supplied athlete ID.
5. **Preserve VKT permissions.** Resolve authorization server-side for every athlete, programme, feedback and report request. Do not infer access from a UI flag or a previously fetched object.
6. **Publish analysis as an additive artifact.** Return a TwinStride analysis ID, status, summary, key measurements, confidence/quality flags, and a secure report link. If writing a summary into VKT feedback is later desired, implement it as an explicit user action and use VKT's supported mutation and permission rules.
7. **Use asynchronous processing.** Video analysis should return a job/analysis ID immediately, with status polling or a callback/event mechanism. Keep heavy processing out of interactive GraphQL requests.
8. **Keep clinical and measurement safeguards.** Mark single-view observations as 2D or otherwise appropriately limited; include capture quality, model/pipeline version, units, timestamps and uncertainty. Clinical rehabilitation and return-to-sport decisions require qualified human review.

## Proposed initial data contract

A TwinStride analysis record should include:

- `analysis_id` — TwinStride-generated stable identifier
- `source_system` — `velocity-knight-trainer`
- `source_environment` — development/staging/production identifier
- `vkt_athlete_id` — exact source athlete identifier and type
- Optional `vkt_program_id`, `vkt_cycle_id`, `vkt_session_id`, `vkt_competition_id`
- `capture_id`, capture timestamp, camera/view metadata and consent reference
- `status` — queued, processing, needs_review, completed, failed
- `pipeline_version`, metric definitions/units, quality flags and uncertainty
- Secure asset/report references and retention/deletion state
- Reviewer identity, review state and audit timestamps where applicable

Do not copy VKT profile or feedback data into TwinStride unless required for a documented feature and covered by access controls, consent and retention policy.

## Integration risks to resolve before coding

- **Authentication header mismatch:** VKT web's Apollo client currently sends `X-User-Id`, `X-User-Type`, and `X-User-Token` (with a Bearer-prefixed token value) alongside `Authorization`. The Go README examples describe `user_id`, `user_type`, and `token` headers. Confirm the actual middleware contract and normalize it before adding integration calls.
- **GraphQL schema/version:** introspect or retrieve the deployed schema; README examples alone are not sufficient to guarantee all fields or operations.
- **Cycle/session semantics:** confirm how a cycle's assigned programme maps to a completed workout/session, and which identifiers are stable across environments.
- **Backend reachability:** confirm the configured deployed GraphQL URL, CORS policy, token issuance/expiry and server-side integration mechanism.
- **Sensitive video and health-related data:** define explicit consent, purpose limitation, access logging, encryption, deletion and retention before storing athlete footage.
- **No silent source-of-truth conflicts:** agree whether TwinStride reports are linked only, or whether selected summaries can be written back to VKT feedback.

## Phased implementation

### Phase 1 — Read-only context
- Confirm schema and auth contract in a non-production environment.
- Resolve the signed-in athlete on the server.
- Read only the minimum programme/cycle/session context required.
- Display a TwinStride entry point from the existing VKT web UI.

### Phase 2 — Capture and analysis
- Create a TwinStride analysis linked to VKT IDs.
- Upload video directly to controlled object storage using short-lived credentials.
- Process asynchronously and present status, measurement provenance and quality flags.

### Phase 3 — Reviewed results
- Allow the athlete/coach to view results only where authorized.
- Add explicit review/annotation and secure report links.
- Optionally add a user-confirmed VKT feedback summary after the write-back contract is agreed.

### Phase 4 — Validation and operational readiness
- Test cross-athlete access denial, expired sessions, role changes, deletion and audit trails.
- Validate metric repeatability and limitations for each capture setup.
- Complete privacy and clinical safety review before any rehabilitation-related pilot.

## Source files reviewed

- VKT Web: `src/lib/apollo.ts`, `src/types/cycles.ts`, `src/types/feedbacks.ts`, `README.md`
- VKT Go: `README.md`, `schema/schema.go`, `module/cycle/types/cycle.go`, `docs/plans/TRAINING_CYCLES_IMPLEMENTATION_PLAN.md`
- TwinStride: `README.md`, `docs/ARCHITECTURE.md`

This is an integration plan, not a claim that a deployed connection or clinical validation already exists.
