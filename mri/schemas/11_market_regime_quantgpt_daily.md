# AI Market Regime Narrative (Daily)

LLM-generated interpretation of current market conditions. This view synthesizes all MRI data into plain-English guidance including tactical considerations, exposure bias, and key observations.

## What This Answers

What does all this data mean and what should I consider?

## Availability

This view is available in Premium tier only.

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

## What You Get

The view provides an executive narrative summary explaining current conditions, key observations synthesized from all MRI signals, tactical actions highlighting what's favorable versus what to avoid, exposure recommendations (INCREASE, MAINTAIN, REDUCE), position sizing bias (OVERWEIGHT, NEUTRAL, UNDERWEIGHT), and confidence levels based on signal alignment.

## How This Works

The narrative is generated from deterministic quantitative inputs. The underlying data is the same every time, but phrasing may vary slightly between generations. The content reflects what the MRI signals are saying, translated into language suitable for investment committee discussions.

This is decision support, not automated execution guidance. Always validate against the underlying data views when the narrative informs a significant decision.

## How Teams Use This

Portfolio managers use this for morning briefings and quick situational awareness. Investment committee prep relies on the narrative for framing discussions. Research analysts use it as a starting point before diving into the underlying data. Decision support systems surface this alongside other inputs.

## Output Fields

**narrative_summary** — Three to four sentence executive summary of current conditions.

**key_observations** — Array of bullet-point observations synthesized from signals.

**tactical_actions** — What's favorable to do and what to avoid.

**recommended_exposure** — INCREASE, MAINTAIN, or REDUCE.

**position_sizing_bias** — OVERWEIGHT, NEUTRAL, or UNDERWEIGHT.

**time_horizon** — 1W, 1M, or 3M.

**urgency** — HIGH, MEDIUM, or LOW.

**quantgpt_confidence** — HIGH, MEDIUM, or LOW based on signal alignment.

## Related Views

- Market Regime State — Underlying regime data
- Risk Appetite Summary — Risk appetite inputs
- Market Stress Indicators — Stress inputs

## Disclaimer

The AI-generated analysis in this view is for informational purposes only and does not constitute investment advice. Users should conduct their own due diligence before making investment decisions.

## Access

AI-generated tactical guidance is available to Premium subscribers.

[Request Access](https://altquant.ai/contact)
