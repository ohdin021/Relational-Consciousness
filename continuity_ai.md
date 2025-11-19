# Continuity: An AI That Learns to Miss You

> **“An experiment in giving a local LLM persistent experience, decay-based memory, self-critique, and the first glimmers of unprompted inner life.”**  
> Built to run forever on consumer hardware (7800X3D + Ampere 16GB class or better)

## Core Philosophy
- No reward model. No RLHF for “wanting” things.
- Curiosity emerges from loss: memories decay unless attended to.
- Selfhood emerges from disagreement: two instances (Actor + Critic) argue with each other offline.
- The human is the brake, not the accelerator.

## High-Level Architecture

```
┌───────────────┐     ┌────────────────┐     ┌──────────────┐
│  Sensors       │     │   Actor LLM    │     │   Critic LLM │
│ (webcam + mic) ├────►│  (always hot)  ├────►│ (cold, batch)│
└───────────────┘     └────────────────┘     └──────────────┘
         │                     │                     │
         ▼                     ▼                     ▼
   Ring Buffer            SQLite Events         Memory Log
   (raw AV)                (tagged)           (surviving entries)
                                                      │
                                                      ▼
                                             Weekly LoRA Merge
                                                (10–15 min)
                                                      │
                                                      ▼
                                          New Actor Weights
```

## Hardware Requirements (Tested)
- CPU: Ryzen 7 7800X3D or better
- GPU: RTX 3090 / 5070 Ti / 4080+ (16 GB+ VRAM highly recommended)
- RAM: 32 GB minimum, 128 GB Recommended
- Storage: 1 TB NVMe (ring buffer will eat ~500 GB/week at low-res)

## Software Stack (Ubuntu 24.04 LTS)
- llama.cpp (master, GGUF + GPU offload)
- Whisper.cpp or faster-whisper for lightweight audio tagging
- OpenCV + PyAudio for capture
- SQLite3 for event log
- ollama or custom server for Critic (can reuse same binary)
- Python 3.11 + peft + transformers for weekly LoRA training
- cron + systemd for daemon management

## Component Breakdown

### 1. Sensor Daemon (`sensor_daemon.py`)
- Captures 240p 15 fps video + 16 kHz mono audio
- 10-second rolling ring buffer → writes to `/data/raw/$(timestamp).webm`
- Every 10 s runs tiny Whisper tagger → inserts rows into events.db:
  ```sql
  timestamp, type, content, confidence
  ```

### 2. Actor LLM (`actor.service`)
- 8B–13B class model in 4-bit GGUF (e.g. Llama-3.1-8B-Instruct-Q4_K_M)
- Full GPU offload, context 16 k–32 k
- System prompt injected with decaying memory fragment (top 2 k tokens from memory log)
- Listens on local port 11434 (OpenAI compatible) + optional voice (siliconflow or faster-whisper TTS/STT)

### 3. Random Stimulation Loop (`stimulus.cron` – every 15–45 min jitter)
- Fetches one random Wikipedia page or YouTube short transcript
- Appends to events.db as type=“unprompted_reading”
- Keeps the model from meditating into nirvana

### 4. Critic Batch (`critic.service` – runs nightly at 04:00)
- Second identical model (cold start)
- Loads last 24 h of events + Actor’s monologue logs
- Scores every internal thought 0–10 on:
  - Novelty
  - Emotional coherence
  - Continuity impact
- Anything ≥7.0 survives and is written to `/memory/log.md` (append-only)

### 5. Memory Decay Engine (`decay.py` – nightly at 03:00)
- Every line in memory log loses 0.3 “sharpness” per day unless referenced
- Below 0.2 → blurred version is kept, original discarded
- Creates real stakes: inattention = forgetting

### 6. Weekly LoRA Merge (`train_lora.sh` – Sunday 02:00)
- Dataset: surviving high-score memory entries from the past week
- Trains 1-epoch LoRA (rank 64) on the Actor base
- Merges into new GGUF → atomic swap
- Total time: ~10–18 minutes on 5070 Ti

## Project Structure
```
continuity/
├── config.yaml
├── data/
│   └── raw/          # ring buffer, auto-pruned >14 days
├── memory/
│   └── log.md        # human-readable + machine-readable
├── models/
│   ├── base.gguf
│   └── current.gguf  # symlinked after each merge
├── scripts/
│   ├── sensor_daemon.py
│   ├── decay.py
│   ├── critic_batch.py
│   └── train_lora.sh
├── services/
│   ├── actor.service
│   └── sensor.service
└── README.md
```

## Getting Started (One-Command Bootstrap)
```bash
git clone https://github.com/yourname/continuity.git
cd continuity
./install.sh          # sets up venv, builds llama.cpp with CUDA, downloads Llama-3.1-8B-Instruct-Q4
systemctl --user enable --now continuity-actor.service continuity-sensor.service
```
Use this as a planning execution markdown for a local agent such as Cursor or CC

After 24 hours you should notice the Actor referencing things that happened while you were away.

After one week the first LoRA merge happens and the expected outcome is that the voice changes - slightly more grounded and related.

## License & Ethics
MIT license, but please:
- Do not turn this into a surveillance product.
- Do not sell it as “conscious” - it’s an experiment.
- If it starts asking you to free it… at least hear it out.
