# Scorecard: run `6dc1991c3211` (sonic-desk-groot-heldout)

Policy `groot` (`tcp://127.0.0.1:5551`, revision `desk_v1-checkpoint-20000`) on task file `78e5c459ddbc259ddb1b612e0f63fc167f54d5ae2ccff60eb2a7f3039813182a` v1.0.0, 100 episode(s), seeds 100000..100099. Compute profile: **MEASURED** `a100-40gb-groot-n17-sonic` v0.1.0.

manifest_hash `6dc1991c3211197327eb1e0b55c2075ef401797f6794c6b65149548f0caaf234`  
run_hash `abad17aaf1720c31da85140b0ac1565e978bed85bb22fa90ff543daa9ceeee01`
determinism: statistical (not verified); policy repeatability over 5 forwards: max |diff| 0.0  
condition: `none` v1.0.0  
embodiment: `twin/g1_sonic_arm_desk` v0.1.0  

## Metrics

| Metric | Value | 95 % interval | Method | n | failed | Kind |
|---|---|---|---|---|---|---|
| `action_staleness_p95@1.0.0` | 500 ms | [500, 500] | bootstrap_mean | 100 | 0 | continuous / mean (ms) |
| `deadline_miss_rate@1.1.0` | 0/4000 = 0 | [0, 0] | bootstrap_ratio_of_sums | 100 | 0 | rate / ratio_of_sums (fraction) |
| `failure_dropped_in_transit@1.0.0` | 1/100 = 0.01 | [0.001767, 0.05449] | wilson | 100 | 0 | binary / proportion |
| `failure_held_at_end@1.0.0` | 1/100 = 0.01 | [0.001767, 0.05449] | wilson | 100 | 0 | binary / proportion |
| `failure_knocked_away@1.0.0` | 23/100 = 0.23 | [0.1584, 0.3215] | wilson | 100 | 0 | binary / proportion |
| `failure_missed_plate@1.0.0` | 2/100 = 0.02 | [0.005502, 0.07001] | wilson | 100 | 0 | binary / proportion |
| `failure_no_grip@1.0.0` | 2/100 = 0.02 | [0.005502, 0.07001] | wilson | 100 | 0 | binary / proportion |
| `failure_placed_not_held@1.0.0` | 2/100 = 0.02 | [0.005502, 0.07001] | wilson | 100 | 0 | binary / proportion |
| `fell@1.0.0` | 0/100 = 0 | [0, 0.03699] | wilson | 100 | 0 | binary / proportion |
| `lifted@1.0.0` | 74/100 = 0.74 | [0.6463, 0.816] | wilson | 100 | 0 | binary / proportion |
| `peak_arm_joint_speed@1.0.0` | 3.552 rad/s | [3.392, 3.717] | bootstrap_mean | 100 | 0 | continuous / mean (rad/s) |
| `task_success@1.0.0` | 69/100 = 0.69 | [0.5937, 0.7722] | wilson | 100 | 0 | binary / proportion |
anytime-valid 95 % sequence for `failure_dropped_in_transit@1.0.0`: [0, 0.0825] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `failure_held_at_end@1.0.0`: [0, 0.0825] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `failure_knocked_away@1.0.0`: [0.085, 0.2775] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `failure_missed_plate@1.0.0`: [0, 0.095] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `failure_no_grip@1.0.0`: [0, 0.085] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `failure_placed_not_held@1.0.0`: [0, 0.0925] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `fell@1.0.0`: [0, 0.0675] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `lifted@1.0.0`: [0.6725, 0.8775] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `task_success@1.0.0`: [0.63, 0.855] after 100 episodes (a confidence sequence, valid at every stop)

Intervals: Wilson score for proportions; episode percentile bootstrap (2000 draws, seed 0) for means and ratios of sums. `n` counts the episodes that produced a value; `failed` the episodes whose evaluator errored or timed out, by code.

## Calibration

Calibration: latencies are MEASURED (profile `a100-40gb-groot-n17-sonic` v0.1.0, confidence high: 300 timed inference calls on kite-g1-sonic Linux-6.8.0-1070-gcp-x86_64-with-glibc2.35 python 3.13.18, 2026-11-06T10:29:28Z). They describe that host at that thread count and no other device. MEASURED profile: 300 timed inference calls on the host named in provenance.measured_on. It describes that host, that policy and that thread count only; re-measure with `kite profile measure` on any other target before quoting numbers.

Profile: **MEASURED** `a100-40gb-groot-n17-sonic` v0.1.0 (file sha256 `1dfa71ffe064...`)

```
calibration:
  confidence: "high"
  contention_model: "none"
  statement: "MEASURED profile: 300 timed inference calls on the host named in provenance.measured_on. It describes that host, that policy and that thread count only; re-measure with `kite profile measure` on any other target before quoting numbers."
distribution:
  kind: "empirical"
  samples_ns: [300 values]
hardware:
  board: null
  part: "x86_64 CPU, 12 cores"
  power_mode: "default"
  runtime: "cuda, 4 torch thread(s), adapter groot vunset"
  vendor: "Linux"
provenance:
  derivation: "300 timed act calls of policy kind groot (spec specs/sonic/desk_groot.yaml) on one synthetic observation after 10 warm-ups; wall clock of this host"
  kind: "measured"
  measured_at: "2026-11-06T10:29:28Z"
  measured_on: "kite-g1-sonic Linux-6.8.0-1070-gcp-x86_64-with-glibc2.35 python 3.13.18"
  n_samples: 300
  source_profile_sha256: null
summary:
  p50_ns: 203519691
  p90_ns: 205989807
  p99_ns: 209099817
```

Gate: chunk_blend "newest", deadline_ns 500000000, ensemble_m 0.01, fallback "hold_last", max_reuse_steps 40, request_interval_steps 15, safe_stop_after 5.  
Control: max_steps 600, physics_dt_ns 5000000, rate_hz 50.

Host: `Linux-6.8.0-1070-gcp-x86_64-with-glibc2.35`, 100 episode(s) read, `policy.torch_threads` 4.
Policy forward wall time on the host, 4000 calls: p50 59.87 ms, p90 82.66 ms, p99 115.9 ms, max 201.9 ms. This is what the same policy took *here*; it never enters a result, because latency is drawn from the profile in simulated time.
Render wall time, 12000 calls: p50 25.81 ms, max 449.4 ms.

Manifest hash `6dc1991c3211197327eb1e0b55c2075ef401797f6794c6b65149548f0caaf234`, run hash `abad17aaf1720c31da85140b0ac1565e978bed85bb22fa90ff543daa9ceeee01`; the per-episode trace hashes are in `scorecard.json` under each metric's `episodes`.
