# TwinStride AI

**Working product name:** TwinStride AI (name and trademark availability not yet checked)  
**Status:** Product and architecture blueprint  
**Initial focus:** Sprint performance and progressive return-to-sprinting

TwinStride AI is a proposed personalised athletic digital twin platform. It helps athletes and their support teams understand movement, track performance and recovery, and make better-informed training and return-to-sport decisions. It is designed around each athlete's history and context rather than treating one movement style as ideal for everyone.

## Product capabilities

1. **Athlete digital twin** — athlete profile, body-segment measurements, goals, baselines, session history, and recovery context.
2. **Biomechanics** — phone-video capture, pose estimation, quality checks, movement replay, supported kinematic measurements, and longitudinal comparisons.
3. **Performance intelligence** — acceleration, sprint splits, speed maintenance, and training response; maximum velocity only when the measurement setup supports it.
4. **Evidence-linked interpretation** — explain observations, plausible explanations, uncertainty, limitations, and useful follow-up.
5. **Rehabilitation and return to sport** — symptoms, functional tests, running exposure, clinician-defined criteria, and documented human decisions.

## Documentation

- [Product blueprint](docs/PRODUCT_BLUEPRINT.md)
- [Technical architecture](docs/ARCHITECTURE.md)
- [Data model](docs/DATA_MODEL.md)
- [Biomechanics and AI](docs/BIOMECHANICS_AND_AI.md)
- [Rehabilitation and safety](docs/REHABILITATION_AND_SAFETY.md)
- [Development roadmap](docs/ROADMAP.md)

## Proposed technology

- Frontend: React + TypeScript
- 3D visualisation: Three.js
- API: Python + FastAPI
- Pose estimation: evaluate MediaPipe Pose Landmarker
- Multi-view biomechanics: evaluate OpenCap/OpenSim before building a custom pipeline
- Signal processing: NumPy + SciPy
- Structured records: PostgreSQL
- Video and derived assets: S3-compatible object storage
- Background processing: Redis-backed worker queue
- Packaging/deployment: Docker, with K3s as a later target once worker capacity and operational requirements are confirmed

These are proposed choices, not claims that the application has already been implemented.

## Safety and validation

TwinStride AI is a decision-support concept, not an autonomous diagnostic or clearance system. Ordinary phone video does not directly measure ground-reaction forces, muscle forces, or internal tissue loads. Measurements must carry provenance, quality information, and limitations. Diagnosis, rehabilitation progression, and return-to-sport clearance remain the responsibility of appropriately qualified professionals.

A roughly 12-week plan is a prototype and supervised-pilot target, not a promise of clinical validation.

## Current status

This repository begins with product and technical documentation. Implementation, measurement validity, clinical effectiveness, and branding remain to be established.
