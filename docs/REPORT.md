# CartPole report

Replace each **TODO** with your own observations. Keep this report short; link code,
compact results, and images instead of pasting logs.
For each stage, explain which tool or function uses your inputs, where the
results come from, and what uses them next. Point to the relevant code or
documentation; a few sentences or an annotated arrow diagram is enough.

To link a file in Markdown, use `[link text](relative/path/to/file)`.
For example, `[Environment code](../onboarding/env.py)` becomes
[Environment code](../onboarding/env.py). Links are relative to this report's
`docs/` folder; use `../` to reach the repository root.
To display an image, add `!`: `![Model screenshot](../results/model.png)`.

## Setup

- Member: Aaron Li
- OS: Ubuntu 26.04.1 LTS(WSL2) running on Microsoft Windows 11, Version 25H2 (OS Build 26200.9711).
- Setup diagnostic, viewer, and RGB results; any fix needed: All diagnosies passed
- Fork URL and working branch: https://github.com/absgjnn/RL.git onboarding/cartpole
- Before starting, read the [toolchain overview](../resources/toolchain).
  What role does each of MuJoCo, Gymnasium, Stable-Baselines3, and TensorBoard
  play in this exercise? Describe how they work together in your own words: 
  Mujoco simulate the physical properties of the cartpole and the scene. Gymnasium creates a RL interface
  by defining the observations, rewards, actions, resets, terminations and truncations. SB3 is a python library
  that includes the algorithms for RL such as PPO, which trains the policy. The tensorboard displays training
  metrics over time in a RL so we can tell wether the agent is learning. 

## Model and task

### Model (Stage 2)

- Which tool loads `scene.xml` and its included `cartpole.xml`, and what does it
  create from them? Point to the loading call in `scripts/view_model.py`. Which
  tool computes the motion you see in the viewer when a control is applied? 
  Mujoco loads the xml files and compile it into Mjmodel using mujoco.MjModel.from_xml_path, which simulate a cartpole with the scene. 
  Mujoco computes the motions and dynamics when the controls are applied, and the viewer will display
  the resulting motion. 
- Which XML file owns the mechanism, and how does the include connect it to the
  scene? Explain the slide/hinge axes, unactuated pole, box half-extents, and
  degrees versus radians. Link a small model screenshot (`../results/model.png`): **TODO**
  The cartpole file owns the mechanism. It defines the cart, the pole, the joint, and the actuator. The scene defines the simulation environment of the
  cartpole, including the gravity, timestep, camera, light and the floor. The slide/hinge axes limited the motion of the cart on a slide and the rotation 
  axis of the pole. The unactuated pole means there isn't external force acting on the pole. The pole will only affected by the motion of the cart and 、
  the gravity. Degrees and radians are units of the angle, which defines the movement range of the pole. ![Model screenshot](RL/results/model.png)
  

### Environment (Stage 3)

Answer each row in a few sentences, using the source linked in
[Stage 3](ONBOARDING.md#3-environment-setup-and-inspection). Name the function or
setting that supports your answer, and distinguish MuJoCo's role from Gymnasium's.

| Topic        | Question                                                                                                                                                | Your answer |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| Observations | What are the four values returned to the policy, in order and with units? Where do those values come from, and how does `_get_obs()` assemble them?     |
  The four values are cart_position(m), cart_velocity(m/s), pole_angle(rad), and pole_angular_velocity(rad/s). The values come from self.data.qpos and self data.qvel from the 
  Mujoco simulations.      |
| Actions      | What does the action control, what is its allowed range, and how does it reach the motor?                                                               |
  The action controls the actuator. THe control range is -3 to 3. The gear range of the motor is 100 so the force range is -300 to 300N.       |
| Physics      | Which tool computes motion from your XML model, and how does Gymnasium ask it to advance? How much simulated time passes per action?                    |
 Mujoco, it ask it to advance by giving the cart an action. The Time passes per action is 0.02s.    |
| Reset        | What does `reset_model()` change at the start of an episode? How are the starting values randomized, and what does using the same reset seed reproduce? |
  It set the model to the original state and add some random noises so every reset is different. The starting values are randominized through random number generator, and same 
  reset seed reproduce the same initial state.    |
| Reward       | What is the exact reward rule, which code computes it, and does it use the state before or after the action?                                            |
  The reward rule is for every step that the pole stays upright the reward increases by one. env.step computes it it uses the state after the action.    |

- Environment test and random-rollout results; action-range warning: Pass

## Training

- Run name / source commit / training seed: first-run /8277acdcbd606d92fe0b872125dcea80ac9ca690/ 0
- Requested and actual steps / elapsed time / device: 100000, 100352/ 36.86 / cpu
- Configuration and versions: link `RL/results/<second-run>/run.json`
- Learning curve: embed `RL/results/<second-run>/learning-curve.png`

- What do SB3, PyTorch, and `Monitor` each do here? How does SB3 use the environment
  and settings you pass to PPO?
  SB3 is the RL library, which provides the PPO algorathim. PyTorch is the deep learning framework used by SB3 to build and train the neural network. Monitor is used to record
  the information of completed episodes in the gymnasium. 
- What do the curve's axes, averaging, and smoothing mean, and what evidence of
  improvement, variability, or a plateau do you see? **TODO**

## Evaluation

Use reset seeds 10000–10019, deterministic PPO actions, and random action seeds
`reset seed + 20000`. Standard deviations describe episodes (`ddof=0`).

| Agent | Episodes | Return mean ± std | Length mean ± std | Time-limit fraction |
| --- | --- | --- | --- | --- |
| Random | TODO | TODO | TODO | TODO |
| PPO | TODO | TODO | TODO | TODO |

- Per-episode CSV and summary JSON: **TODO** links
- Predetermined PPO rollout (seed 10000): **TODO** briefly describe the behavior you observed
- Limitation or failure, and a specific diagnosis if learning was weak: **TODO**

- Why does this evaluation differ from a training curve, and what can one
  trained seed not establish? What carries over to humanoid soccer, and what is
  missing (for example, contacts, partial observations, or sim-to-real transfer)? **TODO**

## Review

**TODO:** Record exact setup, test, train, and evaluate commands, plus any rendering
environment variable.

Keep the saved policy, TensorBoard logs, and rollout video locally and share them
directly with the RL lead if requested. No file uploads, download links, or GitHub
release are required for these files.

- Automated test results: **TODO**
- Manual model/render/video checks: **TODO**
- Fork PR link and review notes (record merge in the PR): **TODO**

## Feedback

- Did you learn anything from this onboarding? What was new, or what became
  clearer? If little was new, say so: **TODO**
- What improvements would make the onboarding easier to follow or more useful?
  Mention any confusing instructions, missing background, or unnecessary work: **TODO**
