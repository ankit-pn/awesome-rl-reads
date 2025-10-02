# Goal

To replace pretraining cross-entropy objectives with RL-based objectives.

**Reward signal source** – pretrained critic and ground-truth reality.

---

# Points

* Cross-entropy training punishes all but the first.
* AvatarL’s RL rewards them all—if they resonate.

**Traditional Pretraining** – REINFORCE with binary rewards.
**AvataRL** – progressive rewards.

---

# Why You Can’t Skip Pretraining (The Math of Hopelessness)


## First Problem: The Action Space is Huge

If the model is random (untrained):

- `pi_theta(a | s) ~= 1 / 50,000` for every action `a`.

The probability that a correct or plausible token appears in (K = 64) samples is tiny.

Assume only 100 tokens out of 50,000 are *reasonable* in context. Then:

**Probability check**
`P(at least one good token in 64 samples) = 1 - (1 - 100/50,000)^64`

`≈ 1 - (0.998)^64 ≈ 12%`

So **88% of the time, all 64 samples are nonsense → all rewards ≈ 0 → no learning signal.**

---

### After Pretraining

* The top 64 tokens include dozens of plausible options.
* Nearly every rollout gives useful reward signal.
* **Training becomes possible.**

## Second Problem: Win condition is not absolutely clear

In traditional reinforcement learning (e.g., games like chess or Atari), the goal is clear and objective: win, lose, or achieve a measurable score. Rewards are well-defined, making it possible to reinforce correct actions over time.

In language modeling, however, there is no single "correct" next token—even given the same context, many continuations can be valid, fluent, or meaningful depending on intent, style, facts, or creativity. For example, after "The cat...", tokens like "sat", "jumped", or "vanished" may all be appropriate in different contexts.

