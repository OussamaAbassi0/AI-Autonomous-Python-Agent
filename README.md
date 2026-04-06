# AI Autonomous Python Agent

A self-correcting n8n agent that writes Python code, executes it, catches errors, and auto-fixes them — all without human intervention.

---

## The Problem It Solves

Running Python scripts in automation pipelines fails silently or breaks the whole flow when there's a syntax error or runtime exception. This agent closes that loop: it doesn't just run code — it **understands the error and rewrites the code until it works**.

---

## How It Works

```
User request (plain English)
        |
        v
n8n Workflow
        |
        |-- [1] Code Writer node (GPT-4o)
        |       Generates Python script from prompt
        |
        |-- [2] Code Executor node
        |       Runs the script in a sandboxed environment
        |
        |-- [3] Error Detector node
        |       Did it fail? --> YES --> back to [1] with error context
        |                    --> NO  --> return result to user
        |
        v
Result delivered (max 5 retry loops)
```

**The self-correction loop:** On failure, GPT-4o receives the original prompt + the generated code + the exact error traceback, and rewrites the script. This continues until the code runs successfully or the retry limit is reached.

---

## What It Can Do

- Generate and run data transformation scripts
- Automate file processing (CSV parsing, JSON manipulation, Excel exports)
- Call external APIs via Python requests
- Perform calculations, regex processing, and data validation
- Any task expressible in plain English that maps to Python

---

## Stack

| Component | Technology |
|-----------|-----------|
| Orchestration | n8n (self-hosted or cloud) |
| AI Engine | OpenAI GPT-4o |
| Code Execution | n8n Code node (sandboxed Python) |
| Retry Logic | n8n conditional branches (max 5 loops) |

---

## Setup

### Prerequisites

- n8n instance (self-hosted or [n8n.cloud](https://n8n.io))
- OpenAI API key

### Import the workflow

1. Download `workflow.json` from this repo
2. In your n8n dashboard: **Import** → select the file
3. Add your `OPENAI_API_KEY` in the n8n credentials manager
4. Activate the workflow

### Trigger

Send a POST request to the webhook URL with:

```json
{
  "task": "Read the CSV file at /data/sales.csv and return the top 5 rows by revenue as JSON"
}
```

---

## Example Output

**Input:** `"Calculate the first 20 Fibonacci numbers and return them as a list"`

**Iteration 1 — Code generated:**
```python
def fibonacci(n):
    fib = [0, 1]
    for i in range(2, n):
        fib.append(fib[i-1] + fib[i-2])
    return fib[:n]

print(fibonacci(20))
```

**Result:** `[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181]` ✅

---

## Status

![Status](https://img.shields.io/badge/status-stable-brightgreen?style=flat-square)
![n8n](https://img.shields.io/badge/n8n-compatible-EA4B71?style=flat-square)
![GPT-4o](https://img.shields.io/badge/GPT--4o-powered-412991?style=flat-square&logo=openai)

---

## License

MIT

---

**Built by [Oussama Abassi](https://github.com/OussamaAbassi0)** — AI Automation Architect  
[![Portfolio](https://img.shields.io/badge/Portfolio-Visit-00c3ff?style=flat-square)](https://oussama-dev-blue.vercel.app)
