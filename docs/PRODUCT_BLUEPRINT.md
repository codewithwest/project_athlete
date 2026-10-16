# Product Blueprint

## Product definition

TwinStride AI is a personalised athletic digital twin platform for movement understanding, performance development, training-response interpretation, and rehabilitation/return-to-sport support.

- **Initial sport:** sprinting.
- **Initial performance workflows:** acceleration, sprint splits, speed maintenance, and longitudinal comparison.
- **Capture:** phone-first; single-view screening plus calibrated, synchronised multi-view capture for supported 3D analysis.
- **Initial rehabilitation workflow:** proposed hamstring-strain return-to-sprinting workflow, to be designed with qualified clinicians and validated before clinical use.
- **Deployment direction:** containerised services; target the existing K3s environment only after worker capacity, storage, security, and operations are verified.

## Product principles

1. **Individual first:** compare athletes to relevant personal baselines. Population norms provide context, not universal ideals.
2. **Evidence before advice:** link interpretations to measurements, methods, and credible sources.
3. **Separate observation from inference:** distinguish measured or estimated values from possible explanations and human decisions.
4. **Human accountability:** coaches own programming; qualified clinicians own diagnosis, clinical progression, and clearance.
5. **Longitudinal continuity:** preserve session history, baseline versions, and analysis versions.
6. **Quality-aware outputs:** suppress or qualify results when capture quality is inadequate.
7. **Privacy by design:** apply consent, access controls, auditability, retention rules, and deletion workflows to video and health-related records.

## Core modules

### Athlete digital twin
A versioned athlete profile containing relevant anthropometrics, segment measurements, goals, baselines, session history, training exposures, injury episodes, and recovery context.

### Biomechanical analysis
Video ingestion, capture-quality checks, pose estimation, supported 3D reconstruction, stride-event detection, kinematic features, and comparable-session analysis.

### Performance intelligence
Sprint split timing, acceleration, speed maintenance, repeatability, training exposure, and trends. Only report maximum velocity when supported by a suitable measurement method.

### General interpretation
An evidence-linked reasoning layer that explains what changed, what might explain it, how certain the evidence is, what is missing, and what follow-up could help.

### Rehabilitation and return to sport
Injury-specific plans, symptoms, functional test results, running exposure, clinician-set criteria, review workflows, and recorded human-authorised decisions.

## Athlete Intelligence Workspace

- Overview
- Digital Twin / 3D Replay
- Performance Lab
- Rehabilitation
- AI Insights
- Athlete History and Reports

Use role-specific access for athletes, coaches, clinicians, sports scientists, and organisation administrators.

## MVP non-goals

- Autonomous diagnosis or return-to-competition clearance
- Guaranteed injury prediction or prevention
- Inferring ground-reaction force or tissue loading from ordinary video
- Treating one movement pattern as ideal for all athletes
- Presenting a single opaque performance or injury-risk score as definitive
- Replacing professional judgement

## MVP acceptance criteria

- Authorised users can create athlete profiles and record consent.
- Sessions can be created, videos uploaded, and capture-quality checks reviewed.
- Metrics include units, provenance, pipeline version, and quality status.
- Sessions can be compared when task and conditions are sufficiently comparable.
- Interpretations link to source evidence and state uncertainty.
- Clinical progression decisions require an authorised human reviewer.
- Access and important changes are auditable.
