# AI Retraining Guide

> How the AI Assistant improves over time through client feedback, and how to
> retrain the model with new data.

---

## The Feedback Loop

The AI Assistant gets better through a self-improving cycle:

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  1. Client asks a question          ──► AI responds          │
│  2. Client rates the response       ──► 👍 or 👎            │
│  3. Admin reviews feedback          ──► Approve / Reject     │
│  4. Export merged training data     ──► Download JSONL        │
│  5. Retrain on Google Colab         ──► New LoRA adapter      │
│  6. Upload new LoRA to S3           ──► Clients auto-update   │
│                                                              │
│  Clients download the new LoRA on next startup — no manual   │
│  action needed. The model is now smarter.                    │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## Step 1: Review Feedback

Open the **CrespoGuard Admin Portal** and navigate to **AI Feedback**.

The dashboard shows all client feedback:

| Column | Description |
|--------|-------------|
| **Date** | When the feedback was submitted |
| **License** | Which client submitted it |
| **Query** | What the client asked |
| **Response** | What the AI answered |
| **Rating** | 👍 Positive or 👎 Negative |
| **Correction** | Admin-provided correct answer (for negatives) |
| **Status** | Pending / Approved / Rejected |

### Reviewing Negative Feedback

For each 👎 response:

1. Read the query and the AI's response
2. Determine if the response was actually wrong
3. If wrong: write the **correct answer** in the correction field and click **Approve**
4. If the client was mistaken: click **Reject**

### Reviewing Positive Feedback

Positive feedback (👍) is automatically included as reinforcement — the AI's
original response becomes a training example. No admin action needed unless you
want to reject a false positive.

---

## Step 2: Export Training Data

Click **Export Training Data** in the admin dashboard. This generates a merged
JSONL file containing:

1. **Base training data** — the original 500+ Q&A pairs
2. **Approved corrections** — admin-verified fixes for wrong answers
3. **Positive reinforcement** — client-confirmed good responses

The export is a single `.jsonl` file ready for fine-tuning.

!!! tip "Deduplication"
    The export process automatically deduplicates entries. If a correction
    contradicts a base training pair, the correction takes priority.

---

## Step 3: Retrain on Google Colab

1. Open the training notebook: `training/CrespoGuard_AI_FineTune.ipynb`
2. Upload to [Google Colab](https://colab.research.google.com)
3. Select **T4 GPU** runtime (free tier works)
4. Upload the exported JSONL file
5. Click **Run All**

The notebook handles:

- Loading the base Qwen2.5-1.5B model
- Applying QLoRA (4-bit quantization + LoRA adapters)
- Training on your data (~20-30 minutes on T4)
- Exporting the trained LoRA adapter as a GGUF file

!!! warning "GPU runtime required"
    Make sure you select a **GPU runtime** (T4 or better) in Colab. CPU training
    will take hours instead of minutes.

---

## Step 4: Deploy the New Model

After training completes, download the LoRA GGUF file from Colab.

### Option A: Upload to S3 (Production)

Upload the new LoRA to the CrespoGuard S3 bucket. All clients will automatically
download it on their next startup.

```bash
aws s3 cp crespoguard-lora.gguf s3://your-bucket/models/crespoguard-lora.gguf
```

### Option B: Test Locally First

Copy the LoRA file directly to your test server:

```
CrespoGuard/models/crespoguard-lora.gguf
```

Restart the loader to pick up the new model.

---

## Training Data Format

Each line in the JSONL file is a conversation in ChatML format:

```json
{"messages": [{"role": "system", "content": "You are CrespoGuard AI..."}, {"role": "user", "content": "set exp to 5x"}, {"role": "assistant", "content": "{\"changes\":[{\"module\":\"server_rates\",\"setting\":\"exp_rate\",\"value\":5.0}],\"explanation\":\"Setting EXP multiplier to 5x.\"}"}]}
```

### Writing Good Training Pairs

| Do | Don't |
|----|-------|
| Use natural language variations ("5x", "500%", "five times") | Always use the same phrasing |
| Include the full JSON response format | Skip the `changes` array |
| Cover edge cases (invalid modules, security modules) | Only train on happy paths |
| Add troubleshooting Q&A pairs | Only train on config changes |
| Keep explanations concise (1-2 sentences) | Write paragraphs of explanation |

---

## When to Retrain

| Signal | Action |
|--------|--------|
| 10+ approved corrections accumulated | Retrain to incorporate fixes |
| New modules added to CrespoGuard | Add training pairs for new modules, retrain |
| Clients consistently misphrase requests | Add those phrasings to training data |
| Major config structure change | Update base training data, retrain |
| Quarterly maintenance | Export + retrain as routine upkeep |

!!! note "Incremental improvement"
    Each retraining cycle builds on all previous corrections. The model doesn't
    forget old training data — it adds new knowledge on top.

---

## Files

| File | Purpose |
|------|---------|
| `training/crespoguard_finetune.jsonl` | Base training data (500+ pairs) |
| `training/CrespoGuard_AI_FineTune.ipynb` | Google Colab notebook |
| `training/finetune.py` | CLI fine-tuning script (alternative to Colab) |

---

## Next Steps

- [AI Overview](OVERVIEW.md) — architecture, safety, and requirements
- [Usage Guide](USAGE.md) — how to use the AI Assistant
