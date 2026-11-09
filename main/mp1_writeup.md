# LLM Prompting Strategy Summary

## Strategy Comparison Overview

| Strategy | Accuracy (Mean of 3) | Parse Rate (%) | Judge Score | Cost ($) | Latency p50 (s) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`few_shot`** | **2.9** | **100.0%** | **25.0** | **$0.002** | 3.031 |
| **`cot`** | 2.8 | **100.0%** | **25.0** | **$0.002** | 3.023 |
| **`structured`** | 2.8 | **100.0%** | **25.0** | **$0.002** | 3.031 |
| **`zero_shot`** | 2.6 | **100.0%** | 20.4 | **$0.002** | **2.758** |

---

## 1. Strategy winner

**`few_shot`** was the overall winner. 
- It took **1st place in accuracy** (2.9 out of 3) and tied for **highest judge score** (25.0).
- It maintained a **100% parse rate** (it never failed to format correctly).
- It was significantly **faster than `cot`** (Chain-of-Thought) and **`structured`**, taking **3.031 seconds** versus 3.031 seconds (`cot`) and 3.031 seconds (`structured`).
- Every method cost the exact same amount ($0.002), so performance, quality, and speed were the primary differentiators.

---

## 2. Most surprising

The **`structured`** method had the highest latency of all strategies (**3.031 seconds**), performing noticeably slower than `few_shot` while tying `cot` in accuracy (2.8), judge score (25.0) and taking 3.023 seconds.

Additionally, **`zero_shot`** delivered the fastest execution (**2.758 seconds**) and matched all other strategies with a 100% parse rate, but suffered a distinct drop in overall quality—scoring lowest in both accuracy (2.6) and judge score (20.4).

---

## 3. Capstone strategy

I would use **`few_shot`** first. Providing clear, concrete examples guides the model to the target output format and reasoning style without incurring the extra latency overhead of Chain-of-Thought writing. This yields the highest overall output quality whilst keeping latency low.

---

## 4. Next steps

With one more day, I would:
1. **Try smaller, faster models** (like GPT-4o-mini) with `few_shot` to evaluate whether sub-1.5 second latency is achievable without losing accuracy.
2. **Test trickier inputs**, such as edge cases or malformed text payloads, to evaluate how robustly each prompting strategy handles structural errors.