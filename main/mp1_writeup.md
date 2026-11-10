# LLM Prompt Strategy Summary

## 1. Strategy winner

**`few_shot`** was the overall winner. 
- It took **1st place in accuracy** (2.9 out of 3) and tied for **highest judge score** (25.0).
- It maintained a **100% parse rate** (it never failed to format correctly).
- It was significantly **faster than `cot`** (Chain-of-Thought) and **`structured`**, taking **2.878 seconds** versus 3.023 seconds (`cot`) and 3.031 seconds (`structured`).
- Every method cost the exact same amount ($0.002), so performance, quality, and speed were the primary differentiators.

---

## 2. Most surprising

The **`structured`** method had the highest latency of all strategies (**3.031 seconds**), performing noticeably lesser than `few_shot`  in accuracy (2.8), slower with 3.023 seconds and tying judge score (25.0).

Additionally, **`zero_shot`** delivered the fastest execution (**2.758 seconds**) and matched all other strategies with a 100% parse rate, but suffered a distinct drop in overall quality—scoring lowest in both accuracy (2.6) and judge score (20.4).

---

## 3. Capstone strategy

I would use **`few_shot`** first. Providing clear, concrete examples guides the model to the target output format and reasoning style without incurring the extra latency overhead of Chain-of-Thought writing. This yields the highest overall output quality whilst keeping latency low.

---

## 4. Next steps

With one more day, I would:
1. **Try smaller, faster models** (like GPT-4.1 Nano) with `few_shot` to evaluate whether sub-1.5 second latency is achievable without losing accuracy.
2. **Test trickier inputs**, such as edge cases or malformed text payloads, to evaluate how robustly each prompting strategy handles structural errors.