# PersistenceGuard: repeated-change decision logic

**Source:** NeuroSynIQ PersistenceGuard TypeScript implementation, supplied 17 October 2026.

**Important:** These are verbatim excerpts, not standalone runnable modules. Original line numbers are included for traceability.

## Original source lines 74–94

```typescript
  // Count consecutive MEASURED_CHANGE sessions from the latest session backwards
  let consecutiveChangeCount = 0;
  for (let i = classifications.length - 1; i >= 0; i--) {
    if (classifications[i] === 'MEASURED_CHANGE') {
      consecutiveChangeCount++;
    } else {
      break;
    }
  }

  // Check if latest is WITHIN_BASELINE but an earlier comparable session had MEASURED_CHANGE
  const latestClassification = classifications[classifications.length - 1];
  const hadPriorChangeBrokenByBaseline =
    latestClassification === 'WITHIN_BASELINE' &&
    classifications.slice(0, -1).some(c => c === 'MEASURED_CHANGE');

  // Show up to the last 5 comparable sessions in chronological order
  const recentSlice = classifications.slice(-5);
  const sequenceLabels: ('NORMAL' | 'CHANGE')[] = recentSlice.map(c =>
    c === 'MEASURED_CHANGE' ? 'CHANGE' : 'NORMAL'
  );
```

## Original source lines 96–116

```typescript
  if (consecutiveChangeCount === 0) {
    if (hadPriorChangeBrokenByBaseline) {
      return {
        metricKey,
        metricLabel,
        hasComparableSessions: true,
        comparableSessionsCount: classifications.length,
        consecutiveChangeCount: 0,
        hadPriorChangeBrokenByBaseline: true,
        sequenceLabels,
        level: 'within_baseline',
        statusText: 'Temporary measured change — persistence not established',
        shortHeading: 'Within baseline pattern',
        shortSubtitle: 'Temporary measured change — persistence not established (returned to baseline).',
        whyLines: [
          `Today's ${metricLabel} measurement is within the established MotorPrint.`,
          'A previous comparable session showed a measured change, but returning to within baseline broke the consecutive-change sequence.',
          'Therefore: PersistenceGuard classified the earlier difference as a temporary measured change — persistence not established.',
          'NeuroSynIQ does not determine the medical cause of the change.',
        ],
      };
```

## Original source lines 140–160

```typescript
  if (consecutiveChangeCount === 1) {
    return {
      metricKey,
      metricLabel,
      hasComparableSessions: true,
      comparableSessionsCount: classifications.length,
      consecutiveChangeCount: 1,
      hadPriorChangeBrokenByBaseline: false,
      sequenceLabels,
      level: 'single_change',
      statusText: 'Change observed — persistence not established',
      shortHeading: 'Change observed',
      shortSubtitle: 'Persistence not yet established.',
      whyLines: [
        `Today's ${metricLabel} measurement differs from the established MotorPrint.`,
        'A measured change was observed, but there are not yet enough repeated sessions to establish persistence.',
        'Therefore: PersistenceGuard classified this as change observed — persistence not established.',
        'NeuroSynIQ does not determine the medical cause of the change.',
      ],
    };
  }
```

## Original source lines 162–182

```typescript
  if (consecutiveChangeCount === 2) {
    return {
      metricKey,
      metricLabel,
      hasComparableSessions: true,
      comparableSessionsCount: classifications.length,
      consecutiveChangeCount: 2,
      hadPriorChangeBrokenByBaseline: false,
      sequenceLabels,
      level: 'repeated_change',
      statusText: 'Repeated change observed — monitoring recommended',
      shortHeading: 'Repeated change observed',
      shortSubtitle: 'Additional monitoring recommended.',
      whyLines: [
        `Today's ${metricLabel} measurement differs from the established MotorPrint.`,
        'Similar measured changes were observed in 2 consecutive comparable sessions.',
        'Therefore: PersistenceGuard classified this as a repeated change observed — monitoring recommended.',
        'NeuroSynIQ does not determine the medical cause of the change.',
      ],
    };
  }
```

## Original source lines 184–204

```typescript
  // 3 or more consecutive comparable sessions
  return {
    metricKey,
    metricLabel,
    hasComparableSessions: true,
    comparableSessionsCount: classifications.length,
    consecutiveChangeCount,
    hadPriorChangeBrokenByBaseline: false,
    sequenceLabels,
    level: 'persistent_change',
    statusText: 'Persistent measured change',
    shortHeading: 'Persistent measured change',
    shortSubtitle: `Change observed across ${consecutiveChangeCount >= 3 ? 3 : consecutiveChangeCount} consecutive comparable sessions.`,
    whyLines: [
      `Today's ${metricLabel} measurement differs from the established MotorPrint.`,
      `Similar measured changes were observed in ${consecutiveChangeCount} consecutive comparable sessions.`,
      'Therefore: PersistenceGuard classified this as a persistent measured change.',
      'NeuroSynIQ does not determine the medical cause of the change.',
    ],
  };
}
```

## Engineering explanation

The code counts consecutive measured changes from the newest session backward. It distinguishes a single observed change, two repeated changes, and three or more consecutive changes. It also recognizes when a prior change is followed by a within-baseline session. The full function includes additional result fields and explanatory text not all reproduced here.

**Safety:** These classifications describe measured differences, not diagnoses or medical causes.
