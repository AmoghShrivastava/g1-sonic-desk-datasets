# Scorecard: run `ae18dc4cb7bd` (sonic-desk-groot)

Policy `groot` (`tcp://127.0.0.1:5550`, revision `desk_v1-checkpoint-20000`) on task file `78e5c459ddbc259ddb1b612e0f63fc167f54d5ae2ccff60eb2a7f3039813182a` v1.0.0, 100 episode(s), seeds 0..99. Compute profile: **MEASURED** `a100-40gb-groot-n17-sonic` v0.1.0.

manifest_hash `ae18dc4cb7bda5951484b1580a112f6dfd80de90c39f756a28bb874d2531d761`  
run_hash `2f2569b62ce65f0acb7da21817bb4a6470b6c3125172406b01cd9c9d9161f077`
determinism: statistical (not verified); policy repeatability over 5 forwards: max |diff| 1.185937523841858  
condition: `none` v1.0.0  
embodiment: `twin/g1_sonic_arm_desk` v0.1.0  

## Metrics

| Metric | Value | 95 % interval | Method | n | failed | Kind |
|---|---|---|---|---|---|---|
| `action_staleness_p95@1.0.0` | 500 ms | [500, 500] | bootstrap_mean | 100 | 0 | continuous / mean (ms) |
| `deadline_miss_rate@1.1.0` | 0/4000 = 0 | [0, 0] | bootstrap_ratio_of_sums | 100 | 0 | rate / ratio_of_sums (fraction) |
| `fell@1.0.0` | 0/100 = 0 | [0, 0.03699] | wilson | 100 | 0 | binary / proportion |
| `lifted@1.0.0` | 99/100 = 0.99 | [0.9455, 0.9982] | wilson | 100 | 0 | binary / proportion |
| `task_success@1.0.0` | 98/100 = 0.98 | [0.93, 0.9945] | wilson | 100 | 0 | binary / proportion |
anytime-valid 95 % sequence for `fell@1.0.0`: [0, 0.0675] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `lifted@1.0.0`: [0.9175, 1] after 100 episodes (a confidence sequence, valid at every stop)
anytime-valid 95 % sequence for `task_success@1.0.0`: [0.905, 1] after 100 episodes (a confidence sequence, valid at every stop)

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
Policy forward wall time on the host, 4000 calls: p50 198.6 ms, p90 200.1 ms, p99 202 ms, max 369.3 ms. This is what the same policy took *here*; it never enters a result, because latency is drawn from the profile in simulated time.
Render wall time, 60000 calls: p50 1.471 ms, max 2.207 ms.

Manifest hash `ae18dc4cb7bda5951484b1580a112f6dfd80de90c39f756a28bb874d2531d761`, run hash `2f2569b62ce65f0acb7da21817bb4a6470b6c3125172406b01cd9c9d9161f077`; the per-episode trace hashes are in `scorecard.json` under each metric's `episodes`.
