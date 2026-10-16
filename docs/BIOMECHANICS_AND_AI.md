# Biomechanics and AI Interpretation

## Measurement pipeline

1. Validate codec, duration, timestamps, dropped frames, exposure, body visibility, and camera metadata.
2. Extract 2D or 3D landmarks using a versioned pose model.
3. For multi-view reconstruction, synchronise cameras and validate calibration first.
4. Map movement to a documented anatomical model where supported.
5. Detect stride phases and task segments; distinguish acceleration from upright sprinting.
6. Calculate explicitly defined kinematic and timing metrics.
7. Attach provenance, quality flags, and known limitations.
8. Compare sessions only when task, speed, surface, footwear, and capture setup are sufficiently comparable.

## Initial features to evaluate

- Hip, knee, ankle, pelvis, and trunk kinematics where view and model support them
- Stance and flight timing where temporal resolution permits
- Step frequency and estimated stride length with the method stated
- Left-right differences and within-session variability
- Sprint splits from an appropriate timing method
- Changes across repetitions and sessions at comparable task demands

Ordinary phone video does not directly measure ground-reaction force, muscle force, tissue load, or precise internal joint loading. Musculoskeletal estimates require an appropriate model, suitable inputs, and validation.

## Individual baselines

Maintain separate baselines by task and phase. Acceleration mechanics should not be compared naively with upright maximal-velocity mechanics. Record the number of valid sessions, conditions, variability, date, and baseline version.

Flag changes only when they can be distinguished from known measurement noise and ordinary within-athlete variability. A particular foot strike, forward lean, or asymmetry is not automatically pathological.

## Interpretation architecture

1. **Deterministic analytics:** calculate metrics, deltas, trends, and uncertainty.
2. **Evidence retrieval:** retrieve approved sports-science references and applicable protocol documents.
3. **Language-model synthesis:** explain observations and plausible meanings using only supplied evidence, stating uncertainty and missing information.

The model must not invent measurements, sources, diagnoses, causal claims, or probabilities. It should identify competing explanations and suggest follow-up measurement or professional review when evidence is insufficient.

## Structured interpretation contract

```json
{
  "observations": [],
  "comparisons": [],
  "possible_explanations": [],
  "limitations": [],
  "follow_up_questions": [],
  "suggested_next_steps": [],
  "evidence_references": [],
  "requires_professional_review": true
}
```

Populate fields only when supported by available evidence. This is an interface contract, not a guarantee of correctness.

## Validation

- Compare measurements with an appropriate reference system for the intended use.
- Assess test-retest reliability and minimum detectable change.
- Include varied body types, skin tones, clothing, lighting, phone models, and sprint speeds.
- Report failure rates and confidence calibration, not only average accuracy.
- Validate performance measures against suitable timing equipment.
- Have qualified practitioners review usability and clinical workflow.
- Conduct prospective studies before making clinical-outcome or injury-prediction claims.
