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

If the model is random (untrained):

[
\pi_{\theta}(a \mid s) \approx \frac{1}{50{,}000} \quad \forall a
]

The probability that a correct or plausible token appears in (K = 64) samples is tiny.

Assume only 100 tokens out of 50,000 are *reasonable* in context. Then:

[
P(\text{at least one good token in 64 samples})
= 1 - \left(1 - \frac{100}{50{,}000}\right)^{64}
]

[
\approx 1 - (0.998)^{64} \approx 12%
]

So **88% of the time, all 64 samples are nonsense → all rewards ≈ 0 → no learning signal.**

---

# After Pretraining

* The top 64 tokens include dozens of plausible options.
* Nearly every rollout gives useful reward signal.
* **Training becomes possible.**

