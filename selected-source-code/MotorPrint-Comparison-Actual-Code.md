# MotorPrint: comparison with personal baseline

**Source:** NeuroSynIQ PersistenceGuard TypeScript implementation, supplied 17 October 2026.

**Important:** These are verbatim excerpts, not standalone runnable modules. Original line numbers are included for traceability.

## Original source lines 236–258

```typescript
  // 1. Evaluate Comparable Tap Sessions (same hand as latest tap session, valid totalTaps > 0)
  let tapResult = emptyTap;
  if (motorPrint.tap && tapSessions.length > 0) {
    const latestTap = tapSessions[0];
    const comparableTaps = tapSessions
      .filter(s => s && typeof s.totalTaps === 'number' && s.totalTaps > 0 && s.hand === latestTap.hand)
      .slice()
      .sort((a, b) => a.timestamp - b.timestamp);

    const tapBaselineVal = Math.round(motorPrint.tap.typicalTapCount.mean);
    const tapStdDev = motorPrint.tap.typicalTapCount.stdDev || 2;
    const threshold = Math.max(1, Math.round(tapStdDev));

    const classifications: SessionClassification[] = comparableTaps.map(s => {
      if (motorPrint.selectedTapSessionIds.includes(s.id)) {
        return 'WITHIN_BASELINE';
      }
      const diff = s.totalTaps - tapBaselineVal;
      return Math.abs(diff) <= threshold ? 'WITHIN_BASELINE' : 'MEASURED_CHANGE';
    });

    tapResult = buildMetricPersistenceResult('tap', 'Tap', classifications);
  }
```

## Original source lines 260–288

```typescript
  // 2. Evaluate Comparable Reaction Sessions (valid totalTrials > 0 and averageReactionTimeMs > 0)
  let rxResult = emptyRx;
  if (motorPrint.reaction && reactionSessions.length > 0) {
    const comparableRx = reactionSessions
      .filter(
        s =>
          s &&
          typeof s.totalTrials === 'number' &&
          s.totalTrials > 0 &&
          typeof s.averageReactionTimeMs === 'number' &&
          s.averageReactionTimeMs > 0
      )
      .slice()
      .sort((a, b) => a.timestamp - b.timestamp);

    const rxBaselineVal = Math.round(motorPrint.reaction.typicalAvgReactionTime.mean);
    const rxStdDev = motorPrint.reaction.typicalAvgReactionTime.stdDev || 35;
    const threshold = Math.max(25, Math.round(rxStdDev));

    const classifications: SessionClassification[] = comparableRx.map(s => {
      if (motorPrint.selectedReactionSessionIds.includes(s.id)) {
        return 'WITHIN_BASELINE';
      }
      const diff = Math.round(s.averageReactionTimeMs) - rxBaselineVal;
      return Math.abs(diff) <= threshold ? 'WITHIN_BASELINE' : 'MEASURED_CHANGE';
    });

    rxResult = buildMetricPersistenceResult('reaction', 'Reaction Time', classifications);
  }
```

## Original source lines 290–328

```typescript
  // 3 & 4. Evaluate Comparable Trace Sessions (complete sessions with valid primary metrics)
  let tremorResult = emptyTremor;
  let controlResult = emptyControl;
  if (motorPrint.trace && traceSessions.length > 0) {
    const comparableTraces = traceSessions
      .map(s => ({ session: s, resolved: resolveTraceMetricsFromSession(s) }))
      .filter(item => item.session && item.resolved.hasValidPrimaryMetrics)
      .slice()
      .sort((a, b) => a.session.timestamp - b.session.timestamp);

    const tremorBaselineVal = Math.round(motorPrint.trace.typicalTremorWobble.mean * 10) / 10;
    const tremorStdDev = motorPrint.trace.typicalTremorWobble.stdDev || 1.0;
    const tremorThreshold = Math.max(0.8, tremorStdDev);

    const controlBaselineVal = Math.round(motorPrint.trace.typicalAverageDeviation.mean * 10) / 10;
    const controlStdDev = motorPrint.trace.typicalAverageDeviation.stdDev || 1.5;
    const controlThreshold = Math.max(1, controlStdDev);

    const tremorClassifications: SessionClassification[] = comparableTraces.map(({ session, resolved }) => {
      if (motorPrint.selectedTraceSessionIds.includes(session.id)) {
        return 'WITHIN_BASELINE';
      }
      const todayVal = Math.round((resolved.traceDeviationVariabilityPx ?? 0) * 10) / 10;
      const diff = Math.round((todayVal - tremorBaselineVal) * 10) / 10;
      return Math.abs(diff) <= tremorThreshold ? 'WITHIN_BASELINE' : 'MEASURED_CHANGE';
    });

    const controlClassifications: SessionClassification[] = comparableTraces.map(({ session, resolved }) => {
      if (motorPrint.selectedTraceSessionIds.includes(session.id)) {
        return 'WITHIN_BASELINE';
      }
      const todayVal = Math.round((resolved.meanTraceDeviationPx ?? 0) * 10) / 10;
      const diff = Math.round((todayVal - controlBaselineVal) * 10) / 10;
      return Math.abs(diff) <= controlThreshold ? 'WITHIN_BASELINE' : 'MEASURED_CHANGE';
    });

    tremorResult = buildMetricPersistenceResult('tremor', 'Tremor', tremorClassifications);
    controlResult = buildMetricPersistenceResult('control', 'Movement Control', controlClassifications);
  }
```

## Engineering explanation

The app filters comparable sessions, excludes selected baseline sessions from change classification, calculates thresholds from the established MotorPrint, and classifies later sessions as within baseline or measured change. Separate sections cover tapping, reaction time, and trace-derived tremor and movement-control metrics. These excerpts show comparison logic, not the separate process that establishes or locks MotorPrint.

**Safety:** These classifications describe measured differences, not diagnoses or medical causes.
