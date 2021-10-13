---
description: This section will give you an overview of the Reputation score.
---

# Reputation

## Motivation

Long term performance is key.

While your payouts depend on your performance in a single round, your reputation and rank depends on your performance over 20 rounds.

## Calculation

Your reputation for `corr`, `mmc`, and `fnc` on round `n` is a weighted average of that metric over the past 20 rounds including rounds that are currently resolving.

The weights on each round change as the round progresses. New rounds start low and gain weight as it nears resolution. Resolved rounds have full weight, but lose weight as it ages until it is dropped entirely.

```python
# delta is the difference between the current and target round number
def round_weight(delta, day):
  if delta < 4:
      return (5 * delta + day) / 20
  elif delta >= 16:
      return (5 * (20 - delta) - day) / 20
  else:
      return 1
```

For example, here are the round weights on day 3 for round 204.

![round_weights example](../.gitbook/assets/round_weights_horizontal.png)

## Missing scores

Late and missing submissions in the 20 round window are penalized in order for reputation to be comparable across models.

The first late or missed submission will receive the score equivalent to the `example_predictions`. Subsequent late or missed submissions will receive a very low score of `-0.1`.

To avoid this penalty, automate your weekly submission workflow with [Numerai Compute](https://docs.numer.ai/tournament/compute).
