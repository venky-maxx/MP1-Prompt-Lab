# MP1: Prompt Strategy Lab

An end-to-end evaluation lab benchmarking four LLM prompting strategies—**Zero-Shot**, **Few-Shot**, **Structured / Role-Based**, and **Chain-of-Thought (CoT)**—on extracting structured profile data from job posting snippets.

---

## 📋 Task Overview

The goal is to extract three specific target fields from 10 sample job posting snippets (ranging from clean to intentionally ambiguous text):
* `company`: The hiring entity (String)
* `role`: The target job title (String)
* `years_experience_required`: Minimum experience required (Integer)

All strategies are evaluated under isolated conditions using `gpt-4o-mini` at `temperature=0.0` over **40 total asynchronous LLM calls** ($10 \text{ snippets} \times 4 \text{ strategies}$).

---

## 📁 Project Structure

```text
.
├── data/
│   ├── golden_set.jsonl      # Reference evaluation dataset (ideal extractions)
│   └── job_snippets.jsonl    # Raw job posting snippets (10 test entries)
├── docs/                     # Documentation & optional ADRs
├── main/
│   ├── comparison.md         # Final benchmark summary table across strategies
│   ├── mp1_prompt_lab.ipynb  # Async pipeline (asyncio) executing evaluations & scoring
│   └── mp1_writeup.md        # 1-page reflection answering lab questions
├── .env                      # API keys (OPENAI_API_KEY)
├── .gitignore                # Git ignore rules
├── .python-version           # Python runtime version
├── pyproject.toml            # Project configuration and metadata
├── README.md                 # Main project setup and documentation
├── requirements.txt          # Python dependencies
└── uv.lock                   # Lockfile for reproducible environment (`uv`)
```

---

## ⚙️ Setup & Installation

### 1. Clone the Repository
Clone the project repository to your local workspace and navigate into the project folder:
```bash
git clone https://github.com/venky-maxx/MP1-Prompt-Lab.git
cd MP1-Prompt-Lab
```

### 2. Prerequisites
* Python 3.14.7
* An active OpenAI API Key

### 3. Installation

Using **`uv`** (Recommended):
```bash
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
uv pip install -r requirements.txt
```

Using standard **`pip`**:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 4. Environment Setup
Add your OpenAI API key to `.env` in the root folder:
```env
OPENAI_API_KEY=your-actual-api-key
```

---

## 🚀 Running the Pipeline in VS Code (Jupyter Kernel)

1. Open the project folder in **VS Code**:
   ```bash
   code .
   ```
2. Open the notebook at `main/mp1_prompt_lab.ipynb`.
3. **Select the Jupyter Kernel:**
   * Click **Select Kernel** in the top right corner of the notebook editor (or press `Ctrl+Shift+P` / `Cmd+Shift+P` and choose **Jupyter: Select Interpreter to Start Jupyter Server**).
   * Choose your active virtual environment (`.venv` or `venv`) created during setup.
4. **Execute the Notebook:**
   * Click **Run All** at the top of the notebook UI (or use `Ctrl+Shift+Alt+Enter` / `Cmd+Shift+Alt+Enter`).
   * Alternatively, run cells sequentially from top to bottom.

The pipeline asynchronously executes $40 \text{ total calls}$ ($10 \text{ snippets} \times 4 \text{ strategies}$) using `asyncio`, benchmarking extraction results against `data/golden_set.jsonl`.

---

## 💡 Interpreting Results

Upon completing the notebook execution, review your outputs based on the following key metrics:

* **Accuracy (0 to 3):** Measures deterministic exact matching (case-insensitive, trimmed) against the ground-truth fields:
  * `company`: The hiring organization name
  * `role`: Target job title
  * `years_experience_required`: Integer value of required experience
* **Parse Rate (`parse_success`):** Percentage indicating whether the model returned valid, parsable JSON without syntax errors or Markdown block formatting issues.
* **LLM-as-Judge Score (1 to 25):** Evaluated via `gpt-4o` to assess qualitative correctness and context resolution (especially for ambiguous or informal snippets).
* **Latency (p50):** The median response time (seconds) per strategy. Look for latency inflation in **Chain-of-Thought (CoT)** due to extra reasoning tokens.
* **Cost ($):** Total API token spending per strategy. Evaluate whether the accuracy gains of Few-Shot or CoT justify their higher token overhead compared to Zero-Shot.
---

## 📖 Deliverables & Documentation

* **[`main/mp1_prompt_lab.ipynb`](./main/mp1_prompt_lab.ipynb):** End-to-end execution notebook.
* **[`main/comparison.md`](./main/comparison.md):** Results and performance comparison matrix.
* **[`main/mp1_writeup.md`](./main/mp1_writeup.md):** 1-page technical reflection answering strategy choices, unexpected failures, and capstone applications.
