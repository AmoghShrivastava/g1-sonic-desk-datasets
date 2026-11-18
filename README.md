# G1 SONIC desk datasets

Eight datasets of a Unitree G1 picking up an apple and placing it on a plate, on a simulated copy of a real office desk. Each one has labels for GR00T N1.7 driving NVIDIA's SONIC v1.1 whole-body controller.

They start from NVIDIA's [Arena-G1-Static-PickNPlace-Task](https://huggingface.co/datasets/nvidia/Arena-G1-Static-PickNPlace-Task) demos, which use a different controller (AGILE) and a different scene. Each episode follows one of those demos: in `baseline`, the wrists are a median 0.8 cm (left) and 1.0 cm (right) from the demo relative to the pelvis, the arm joints are within about 2.6 degrees, and neither foot lifts or slides more than 2 cm. `reference.arena_state` holds NVIDIA's joints for every frame, so you can compare for yourself. The `positions` dataset differs from the demos on purpose.

![All variants](docs/variants.png)

[Watch all ten looks playing together](docs/preview.mp4), at half speed. They show one of the cleanest episodes, so the apple lands more gently than in a typical one. NVIDIA's demos move fast at their real 50 Hz, and the datasets keep that speed.

## Datasets

| Dataset | What it changes | Episodes |
| --- | --- | --- |
| [`g1-sonic-desk-apple-baseline`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-baseline) | The reference look, with lighting matched to a photo of the real desk. | 162 |
| [`g1-sonic-desk-apple-evening`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-evening) | Dim, warm light from a low ceiling source. The darkest look in the set. | 162 |
| [`g1-sonic-desk-apple-fluorescent`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-fluorescent) | Cool overhead light with soft shadows. | 162 |
| [`g1-sonic-desk-apple-window`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-window) | Low, hard side light with long shadows across the desk and plate. | 162 |
| [`g1-sonic-desk-apple-rearranged`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-rearranged) | The desk's own items (keyboard, mouse, eraser, spray bottle, wipes, small bottle) moved to new spots. | 160 |
| [`g1-sonic-desk-apple-tools`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-tools) | New objects on the desk: stapler, pliers, measuring tape and alarm clock. | 161 |
| [`g1-sonic-desk-apple-types`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-types) | Four apples, varied by episode: the original, a green one, a yellow one and a scanned real apple. The motions are the same as in the other datasets. | 162 |
| [`g1-sonic-desk-apple-positions`](https://huggingface.co/datasets/kiteml/g1-sonic-desk-apple-positions) | The apple starts up to 5 cm from its original spot, and the motion is adapted to reach it. These motions deliberately differ from NVIDIA's demos. | 165 |

All are LeRobot v2.1 at 50 fps, with one 640 x 480 head camera. `rearranged` and `tools` have fewer episodes because they leave out those where an arm comes within 5 cm of an object.

| Key | Shape | Content |
| --- | --- | --- |
| `observation.images.ego_view` | 480 x 640 x 3, 50 fps, H.264 | Head camera |
| `observation.state` | 43 | Joint positions from the replay. Hands are in index, middle, thumb order |
| `observation.projected_gravity` | 3 | Gravity in the pelvis frame |
| `action.motion_token` | 64 | SONIC v1.1 motion token |
| `teleop.left_hand_joints`, `teleop.right_hand_joints` | 7 each | Dex3 hand targets, in thumb, middle, index order |
| `reference.arena_state` | 43 | NVIDIA's original joint positions for this frame, for comparison. Not used in training |
| `reference.arena_episode`, `reference.arena_frame` | 1 each | The NVIDIA episode and frame this row comes from |

Training uses the `unitree_g1_sonic` modality in Isaac-GR00T:

```bash
python gr00t/experiment/launch_finetune.py --base-model-path nvidia/GR00T-N1.7-3B --dataset-path <dataset> --embodiment-tag UNITREE_G1_SONIC --modality-config-path gr00t/configs/data/embodiment_configs.py
```

## A policy trained and evaluated on the desk

[`kiteml/gr00t-n1.7-g1-sonic-desk-apple`](https://huggingface.co/kiteml/gr00t-n1.7-g1-sonic-desk-apple) (private to
kiteml) is GR00T N1.7 fine-tuned for `UNITREE_G1_SONIC` on the desk. In Kite's closed-loop evaluation it places the
apple in **95%** of episodes in the layout it trained on (190/200), and **69%** when the apple and plate move 12 cm
outside it (69/100; most failures knock the apple away before the grasp). It stays at 92–100% under light, camera,
clutter, noise, mass, friction, servo and delay conditions, and it never fell; a slower model is what breaks it. [`policy/`](policy/) has the results and GIFs of
successes and failures.

![Held-out success](policy/gifs/heldout_success_overview.gif)

### Stress tests: what Kite changed, and what happened

Kite changes one thing at a time and runs the policy 100 times from the same apple and plate positions as its
baseline. Each clip below is one real try, at 2× speed; the label names the change and the condition's score. All
of them in one video: [`policy/stress_tests_grid.mp4`](policy/stress_tests_grid.mp4).

<table>
<tr><td align="center"><img src="policy/gifs/conditions/baseline.gif" width="240"><br><sub>Baseline · 99/100</sub></td><td align="center"><img src="policy/gifs/conditions/light_apple.gif" width="240"><br><sub>Lighter apple · 100/100</sub></td><td align="center"><img src="policy/gifs/conditions/heavy_apple.gif" width="240"><br><sub>Heavier apple · 99/100</sub></td><td align="center"><img src="policy/gifs/conditions/slippery.gif" width="240"><br><sub>Slippery surfaces · 100/100</sub></td></tr>
<tr><td align="center"><img src="policy/gifs/conditions/weak_motors.gif" width="240"><br><sub>Weaker motors · 94/100</sub></td><td align="center"><img src="policy/gifs/conditions/strong_motors.gif" width="240"><br><sub>Stronger motors · 99/100</sub></td><td align="center"><img src="policy/gifs/conditions/motor_delay.gif" width="240"><br><sub>Controller delay 15–30 ms · 93/100</sub></td><td align="center"><img src="policy/gifs/conditions/dim_light.gif" width="240"><br><sub>Dim light · 94/100</sub></td></tr>
<tr><td align="center"><img src="policy/gifs/conditions/model_2x_slower.gif" width="240"><br><sub>Model 2× slower · 69/100</sub></td><td align="center"><img src="policy/gifs/conditions/model_3x_slower.gif" width="240"><br><sub>Model 3× slower · 0/100</sub></td><td align="center"><img src="policy/gifs/conditions/safety_normal.gif" width="240"><br><sub>Normal settings · 48/50</sub></td><td align="center"><img src="policy/gifs/conditions/safety_weak_spot.gif" width="240"><br><sub>Search's weak spot · 41/50</sub></td></tr>
</table>

| Group | Change | Parameter and range | Placed / 100 | Baseline |
|---|---|---|---|---|
| Vision | Low light | light intensity ×0.35–0.60 | 94 | 98 |
| Vision | Side light, shadows | one low, hard side light | 94 | 98 |
| Vision | Camera turned | head camera rotated up to ±4° | 93 | 98 |
| Vision | Clutter | extra objects on the desk | 92 | 98 |
| Vision | State noise | Gaussian noise on the robot state, σ 0.02 | 96 | 98 |
| Vision | Reworded instruction | the task asked in other words | 96 | 98 |
| Dynamics | Slippery surfaces | friction ×0.6–0.9 | 100 | 99 |
| Dynamics | Lighter apple | mass 100–130 g (modelled 150 g) | 100 | 99 |
| Dynamics | Heavier apple | mass 200–250 g | 99 | 99 |
| Dynamics | Waxy apple | apple friction 0.4–0.6 | 99 | 99 |
| Dynamics | Weaker motors | actuator gain ×0.85–0.95 | **94** | 99 |
| Dynamics | Stronger motors | actuator gain ×1.05–1.15 | 99 | 99 |
| Dynamics | Stiff joints | joint damping ×1.2–2.0 | 98 | 99 |
| Dynamics | Controller delay | commands reach the motors 5–15 ms late | 98 | 99 |
| Dynamics | Controller delay | 15–30 ms late | **93** | 99 |
| Timing | Model 1.5× slower | policy answers in ~310 ms instead of ~205 ms (A100) | **66** | 99 |
| Timing | Model 2× slower | ~410 ms | **69** | 99 |
| Timing | Model 3× slower | ~615 ms | **0** | 99 |
| Search | Kite's weak spot | all dynamics at once: friction ×0.6–0.9, apple 100–175 g, apple friction 0.4–0.6, gain ×0.85–1.0, damping ×1.2–2.0, delay 5–30 ms | 41 / 50 | 48 / 50 |

The policy holds up to every single change in the world: no condition costs it more than 6 points, and it never fell.
What breaks it is time. When the model answers 1.5× slower than the A100 serves it, success drops to about two thirds,
and at 3× it never places the apple. The search's weak spot costs 7 of 50 on the same tries, a hint rather than a
finding (exact McNemar p = 0.065).

**Safety at the weak spot** (the same 50 tries per side, re-simulated with contact recording on):

| Check | Normal | Weak spot |
|---|---|---|
| Unwanted touches per try (median) | 1.5 | 8 |
| Idle right hand bumped the desk | 12 / 50 | 39 / 50 |
| Hardest press on anything, worst try | 49 N | 151 N |
| Tries with a joint past its limit | 0 / 50 | 3 / 50 |
| Tries where it hit itself | 0 / 50 | 1 / 50 |
| Apple dropped | 0 / 50 | 1 / 50 |
| Jerkiness (dimensionless, lower is smoother) | 18.8 | 18.8 |

It still succeeds most of the time there, but it gets rougher: the idle hand rests on the desk and presses harder.
[`policy/`](policy/) has the full results.

## Limits

- Only the G1, the original apple and the plate are simulated physically. The moved and added desk items, and the other apples in `types`, are visual only.
- The apple lands harder and rolls further than in NVIDIA's demos, and in some episodes it's still rolling when the episode ends. `meta/provenance.json` lists each episode's `apple_fall_cm`, `apple_roll_cm` and `apple_end_speed_cm_s`, so you can filter on them.
- The body sways more than in NVIDIA's demos, because SONIC balances the robot itself: the pelvis moves about 3.7 cm over a typical `baseline` episode, against 0.4 cm in NVIDIA's demos, and the head camera moves with it.
- Simulation only. Nothing has been tested on a real robot or on NVIDIA's own SONIC deploy.

## Credits

Derived from [nvidia/Arena-G1-Static-PickNPlace-Task](https://huggingface.co/datasets/nvidia/Arena-G1-Static-PickNPlace-Task) (CC-BY-4.0). Motion tokens come from NVIDIA's [SONIC v1.1](https://huggingface.co/nvidia/GEAR-SONIC) (NVIDIA Open Model License). Made by [Kite](https://kiteml.com).
