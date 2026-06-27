##Step 1 — Data Preparation

We loaded 200,000 prediction markets from Polymarket's API. After cleaning and dropping missing values, we worked with 44,491 markets that had all the variables needed for causal analysis.
We created a binary treatment variable from rewardsMinSize — a market is "treated" if it has an active reward mechanism (rewardsMinSize > 0) and "control" if it doesn't. About 8.5% of markets had rewards active.



Step 2 — Causal Graph (DAG)

We built a Directed Acyclic Graph using DoWhy to map the assumed causal relationships. The DAG encodes that liquidity, competitive, uncertainty, and duration are confounders — they affect both whether a market gets rewards AND how much volume it generates. Without controlling for these, any observed difference in volume between rewarded and non-rewarded markets could simply be because rewarded markets were already more liquid or popular.


Step 3 — Backdoor Adjustment

We used backdoor adjustment via linear regression to block all backdoor paths through the confounders. This isolates the pure causal effect of the reward mechanism on trading volume, removing the influence of confounding variables.

Step 4 — Result

The estimated Average Treatment Effect (ATE) is ~181,747. This means markets with an active reward mechanism generate on average 181,747 more units of trading volume than markets without rewards, after controlling for all confounders.
Step 5 — Refutation Tests (Validation)

We ran three tests to validate this result:

Random Common Cause — added a random fake variable as confounder. Effect barely changed (181,746 → 181,743, p=0.94). ✅ Estimate is stable.
Placebo Treatment — replaced real treatment with random noise. Effect collapsed to -599. ✅ The real effect is genuine, not random.
Data Subset — ran on a random subset of data. Effect stayed at 181,241 (p=0.98). ✅ Result is consistent across different samples.

All three refutation tests confirm the causal estimate is robust and not a statistical artifact.##

