# NeuroSynIQ-Science-Fair-2026
NeuroSynIQ | Science Fair 2026 Innovation Project — An AI-assisted neuromotor monitoring prototype that tracks movement changes against a personal baseline to support neurological review.

# NeuroSynIQ
## AI-Assisted Neuromotor Monitoring System

**Science Fair 2026 | Innovation Category**

**Theme:** AI: Engineering a Safer Future

**School:** Lakshmi Girls Hindu College

**Student Researchers:** Eliana Jagesar and Elisa Maharaj

**Project Status:** Research & Development Prototype

**Live Prototype:** https://neurosyniq.web.app/

---

## 1. Project Overview

NeuroSynIQ is an AI-assisted neuromotor monitoring research prototype designed to measure changes in movement performance over time.

The system compares new measurements against an individual's established personal baseline and highlights unusual changes for neurological review.

**Our goal:** Support earlier recognition and investigation of movement changes between medical appointments.

**Important:** NeuroSynIQ does not diagnose neurological diseases, prescribe treatment or replace a medical professional.

## 2. The Problem

Neurological conditions can affect movement, coordination and reaction time. Changes may occur between medical appointments and can be difficult to notice or describe consistently.

NeuroSynIQ explores whether repeated digital measurements, personal baselines and explainable change-detection rules could help clinicians review these changes more systematically.

## 3. Our Engineering Innovation

NeuroSynIQ combines five key components:

| Component | Function |
|---|---|
| Neuromotor Tests | Measures tapping, reaction time and movement control |
| MotorPrint | Establishes and locks an individual's personal baseline |
| PersistenceGuard | Distinguishes isolated, repeated and persistent measured changes |
| WHY? Explanations | Explains differences between current results and baseline measurements |
| Research Lab | Tests the change-detection rules using controlled simulated scenarios |

The innovation is the integration of these components into one functioning research prototype.

## 4. How the Prototype Works

**TEST → PROCESS → ANALYZE → STORE → COMPARE → EXPLAIN**

### Step 1 — Measure

Users complete three digital tasks:

- **Tap Test:** Measures tapping performance over 10 seconds.
- **Reaction Test:** Records five reaction attempts.
- **Trace Test:** Evaluates movement control and path deviation.

### Step 2 — Establish MotorPrint

Three baseline sessions for each test are used to establish an individual's reference measurements.

Once established, the MotorPrint is locked so that subsequent monitoring checks do not automatically change the baseline.

### Step 3 — Compare New Results

New measurements are compared against the established MotorPrint.

### Step 4 — Apply PersistenceGuard

PersistenceGuard uses an explainable, rule-based process:

| Consecutive changed sessions | Interpretation |
|---|---|
| 1 | Change observed; persistence not established |
| 2 | Repeated change observed |
| 3 or more | Persistent measured change |

The purpose is to avoid treating a single unusual result as a confirmed persistent change.

### Step 5 — Explain the Results

The WHY? feature displays the measurements involved in a flagged change and explains the comparison in understandable language.

## 5. Engineering Development and Iterations

NeuroSynIQ was improved through several development stages.

| Date | Engineering improvement |
|---|---|
| 30 September 2026 | Initial Tap, Reaction and Trace tests |
| 3–4 October 2026 | Improved Trace measurements beyond a simple percentage score |
| 4–7 October 2026 | Introduced MotorPrint personal baseline and baseline locking |
| 4–7 October 2026 | Developed PersistenceGuard repeated-change logic |
| 4–7 October 2026 | Added WHY? explanations and Research Lab simulations |

These iterations addressed measurement quality, baseline consistency, explainability and premature persistence classification.

## 6. Testing and Validation

### Controlled 100-Case Simulation

We evaluated PersistenceGuard using 100 computer-generated cases across five scenarios:

- Stable measurements — 20 cases
- Normal variation — 20 cases
- Temporary changes — 20 cases
- Gradual changes — 20 cases
- Persistent changes — 20 cases

### Results

| Outcome | Result |
|---|---|
| Gradual and persistent cases reaching repeated-or-persistent status | 40/40 |
| Gradual and persistent cases strictly confirmed within five sessions | 37/40 (92.5%) |
| Temporary one-off cases incorrectly classified as strictly persistent | 0/20 |
| Gradual cases needing further observation for strict confirmation | 3/20 |

These results demonstrate how the algorithm behaves under controlled simulated conditions.

**They do not establish clinical accuracy, diagnostic performance or patient safety.**

### Real-World Prototype Testing

The prototype was also evaluated using actual tapping measurements to observe differences between faster and deliberately slower tapping performance.

These observations are separate from the computer-generated validation cases and are not clinical studies.

## 7. Humanity Safeguards and Limitations

NeuroSynIQ was designed around the principle:

**AI Assists. Humans Decide.**

The prototype:

- Does not diagnose neurological disease.
- Does not determine the medical cause of a change.
- Does not prescribe treatment.
- Does not replace neurologists or other medical professionals.
- Uses understandable explanations rather than unexplained medical conclusions.

Limitations include fatigue, practice effects, device differences, small personal baselines and delays in confirming gradual changes.

PersistenceGuard is a rule-based algorithm, not a clinically trained AI diagnostic model. AI-assisted tools supported prototype development.

## 8. Engineering Evidence Portfolio

The supporting engineering portfolio contains:

- Engineering design requirements and constraints
- Development and iteration timeline
- System architecture and blueprints
- Actual application screenshots
- Selected code excerpts
- Testing and simulation evidence
- Engineering trade-offs and safety limitations
- Student contributions

**[View the Engineering Evidence Portfolio](NeuroSynIQ_Engineering_Evidence_Portfolio_Judges_Edition.pdf)**

## 9. Student Contributions

### Eliana Jagesar — Software Development & Engineering

Developed and improved the NeuroSynIQ software prototype, including the neuromotor tests, MotorPrint, PersistenceGuard and explanation features, using AI-assisted development tools where applicable.

### Elisa Maharaj — Research, Testing & Presentation

Contributed to project research, testing and evaluation, preparation of the science fair display, and project presentation.

The students' responsibilities are identified separately to acknowledge their actual contributions.

## 10. Live Demonstration

**Try the NeuroSynIQ research prototype:**

https://neurosyniq.web.app/

The live demonstration illustrates the measurement workflow, personal baseline comparison, PersistenceGuard and explainable results.

This application is an educational research prototype and is not intended for clinical use.

---

**NeuroSynIQ — Detect · Analyze · Explain · Support**

*Research & Development Prototype | Science Fair 2026 | Innovation Category*
