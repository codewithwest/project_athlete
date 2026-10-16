# Technical Architecture

## Design goals

Use a modular, API-first architecture. Run video analysis asynchronously and separately from interactive API requests. Keep deterministic measurement and calculation services separate from AI-generated explanations.

## Proposed stack

| Layer | Candidate | Responsibility |
|---|---|---|
| Web client | React + TypeScript | Athlete and professional workspaces |
| 3D viewer | Three.js | Skeleton visualisation, replay, and comparisons |
| API | Python + FastAPI | Authenticated endpoints and workflow orchestration |
| Pose estimation | Evaluate MediaPipe Pose Landmarker | Initial landmark extraction and capture feedback |
| 3D biomechanics | Evaluate OpenCap/OpenSim | Multi-view reconstruction and musculoskeletal modelling |
| Signal processing | NumPy + SciPy | Filtering, event timing, and time-series features |
| Database | PostgreSQL | Profiles, sessions, metrics, decisions, and audit records |
| Object storage | S3-compatible | Original video, derived assets, and reports |
| Job queue | Redis-backed workers | Analysis jobs and report generation |
| Deployment | Docker; later K3s | Repeatable packaging and orchestration |

Before selecting dependencies, verify current maintenance, licensing, hardware requirements, and suitability for the intended use.

## Logical services

1. **API service:** authentication, authorisation, athlete records, sessions, and workflow state.
2. **Capture service:** upload validation, camera metadata, calibration references, consent, and quality gates.
3. **Biomechanics worker:** landmark extraction, reconstruction, event detection, metric calculation, and artefact storage.
4. **Performance analytics:** timing data, baselines, matched-session comparisons, trends, and variability.
5. **Interpretation service:** evidence retrieval, structured synthesis, uncertainty, and review requirements.
6. **Rehabilitation workflow:** versioned plans, clinician-defined criteria, review status, and sign-off.
7. **Audit and operations:** access logs, pipeline versions, monitoring, backups, and incident response.

## Capture modes

### Single-view screening
Useful for selected 2D observations and capture feedback. Do not imply that one uncalibrated view resolves full 3D movement.

### Multi-view assessment
Use synchronised, calibrated views where supported by the chosen pipeline. Store camera geometry, frame rate, synchronisation method, and calibration quality.

High frame rates can help capture sprint events, but exposure, shutter behaviour, image quality, camera placement, and dropped frames also matter.

## Data flow

1. User creates a session and confirms permission to process the recording.
2. Video and capture metadata are uploaded to object storage.
3. API creates an asynchronous processing job.
4. Worker checks quality and extracts landmarks.
5. Supported pipelines calculate metrics and store derived assets.
6. Analytics compare compatible sessions and baselines.
7. Interpretation receives structured evidence with provenance and quality flags.
8. Dashboard presents results, uncertainty, and required reviews.
9. Human decisions and later outcomes are recorded separately.

## Deployment notes

Start with local Docker-based development. For K3s, isolate CPU/GPU-intensive workers from control-plane duties, set resource requests and limits, configure persistent storage, and test backup restoration. Do not assume a low-memory control-plane node can reliably process high-frame-rate video.

## Reliability and security

- Enforce authenticated, role-based access.
- Encrypt traffic and stored sensitive files.
- Use short-lived upload/download URLs where appropriate.
- Validate file type, size, codec, and duration.
- Keep secrets out of source control.
- Version analysis pipelines and metric definitions.
- Monitor queue age, processing latency, failures, and worker resource use.
- Back up structured data and define object-storage retention and recovery procedures.
