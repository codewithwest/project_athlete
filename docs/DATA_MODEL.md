# Data Model

## Principles

- Separate raw evidence, calculated measurements, AI interpretations, interventions, and human decisions.
- Version analysis outputs and clinical decisions; avoid silently rewriting historical records.
- Each derived metric identifies its source recording, method, units, pipeline version, and quality status.
- Store large media and dense frame sequences in object storage or efficient columnar files; keep queryable summaries in PostgreSQL.
- Enforce organisation boundaries and least-privilege access.

## Core entities

| Entity/table | Purpose |
|---|---|
| `users` | Authenticated people and account status |
| `organisations` | Teams, clinics, academies, and tenant boundaries |
| `memberships` | User, organisation, role, and permissions |
| `consents` | Scope, version, timestamp, and withdrawal status |
| `athletes` | Athlete profile and lifecycle status |
| `athlete_segments` | Segment measurements, method, and date |
| `athlete_goals` | Goal, target, timeframe, owner, and status |
| `athlete_baselines` | Versioned reference state and inclusion criteria |
| `sessions` | Athlete, date, task, and session context |
| `recordings` | Session, camera view, frame rate, calibration, and storage key |
| `camera_calibrations` | Camera parameters, method, quality, and timestamp |
| `analysis_runs` | Recording, pipeline version, status, and quality summary |
| `motion_frames` | Analysis, timestamp, landmark coordinates, and confidence |
| `biomechanical_metrics` | Metric name, value, units, side, uncertainty, and provenance |
| `performance_tests` | Test protocol, distance, splits, timing method, and result |
| `training_exposures` | Repetitions, distance, intensity, rest, and workload context |
| `readiness_assessments` | Athlete-reported recovery, soreness, and sleep |
| `injury_episodes` | Clinician-entered diagnosis, side, onset, and status |
| `rehab_plans` | Injury episode, protocol version, and responsible clinician |
| `progression_criteria` | Criterion, threshold/rubric, source, and reviewer |
| `clinical_assessments` | Symptoms, functional tests, and clinician notes |
| `interpretations` | Observations, hypotheses, limitations, evidence links, and model version |
| `evidence_references` | Source, version/date, and relevant identifier |
| `interventions` | Proposed or approved action, author, rationale, and status |
| `progression_decisions` | Decision, rationale, reviewer, timestamp, and plan version |
| `outcome_evaluations` | Follow-up window, comparable outcomes, and limitations |
| `audit_events` | Actor, action, resource, timestamp, and revision |

## Relationships

An athlete has many sessions, goals, baselines, assessments, and injury episodes. A session has one or more recordings and tests. A recording can have multiple versioned analysis runs. An analysis run produces metrics and derived motion artefacts. Interpretations reference specific analysis runs and evidence. Rehabilitation plans have versioned criteria and human-reviewed progression decisions.

## Metric record contract

Every metric should include, where applicable:

- `metric_name`, `value`, and `unit`
- side/segment, task, phase, and timestamp or aggregation window
- source recording and analysis run identifiers
- measurement method and pipeline version
- quality status and uncertainty
- creation timestamp

Do not fabricate uncertainty. If it has not been quantified, mark it unknown and explain the limitation.

## Illustrative metric

```json
{
  "metric_name": "knee_flexion_angle",
  "value": 42.6,
  "unit": "degrees",
  "side": "left",
  "task": "acceleration",
  "timestamp_ms": 1840,
  "measurement_method": "multi_view_3d",
  "pipeline_version": "pose-pipeline-0.1",
  "quality_status": "review_required",
  "uncertainty": null
}
```

The numeric value is illustrative only, not a clinical reference.

## Retention

Define separate retention policies for original video, derived motion, routine metrics, clinical notes, and audit events. Consent withdrawal and deletion must account for backups, legal obligations, research use, and downstream copies.
