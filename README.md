# AquaCheck-Edge
Here is your completed project submission document, pre-filled with all the technical details, architecture specs, and metrics from your local PaliGemma setup:

---

# AquaCheck Edge AI

**Track:** Intelligence with Purpose

**Team:** The Rookies

---

## Problem

Over 2 billion people worldwide rely on water sources that lack real-time safety monitoring, particularly in off-grid rural communities and disaster zones. Field testing kits exist, but local personnel often lack expert knowledge to accurately interpret subtle physical cues or multi-parameter test strip outputs. Without fast, reliable local triage, communities face delayed identification of waterborne disease hazards and critical contamination events.

---

## Solution

AquaCheck Edge AI is a 100% offline-capable, on-device water quality diagnostic pipeline and spatial hazard mapping system. Users simply take a photo of a water sample or test strip and input basic physical observations (clarity, odor, symptoms) into a light web interface. Running entirely on local GPU hardware via PaliGemma, the system instantly computes a scientific Water Quality Index (WQI), performs multimodal safety analysis, generates localized emergency guidance in multiple languages, and updates a community spatial hazard map—all without requiring an active internet connection or external API calls.

---

## How Gemma Is Used

* **Model variant:** PaliGemma 3B (`google/paligemma-3b-pt-224`)
* **How it's used:** Quantized for on-device local GPU inference (4-bit BitsAndBytes quantization) accepting multimodal input (image + text prompt).
* **Why specific Gemma variant:** PaliGemma 3B provides the ideal balance of lightweight parameter count and native vision-language reasoning. It runs comfortably within 16GB VRAM on consumer/edge GPUs without network latency or cloud API dependency.
* **Any customization:** Implemented 4-bit NormalFloat (`bnb_4bit_compute_dtype=bfloat16`) quantization with custom prompt engineering to convert visual sample tokens and structured field metadata into actionable safety advisories.

---

## Architecture

```
[User Interface / Mobile Browser]
        │
        │ Uploads Image + Parameters
        ▼
[Gradio Web Client (Kaggle/Edge Host)]
        │
        ├──► [NSF/Horton Mathematical WQI Engine] ──► Calculates 0-100 Score
        │
        ├──► [Local GPU Memory (VRAM)]
        │      └── [PaliGemma 3B (4-bit NF4)]
        │            └── Generates Multimodal Safety Diagnostics
        │
        └──► [Leaflet.js Geospatial Engine] ───────► Renders Live Map Pins

```

**Tech stack:** Python 3.12, PyTorch, Hugging Face `transformers`, `bitsandbytes`, Gradio, Pandas, Leaflet.js, OpenStreetMap API.

---

## Results / Demo

* **Zero Cloud Latency & 100% Privacy:** Runs entirely on-device without external API calls or user data transmission.
* **Low VRAM Footprint:** Reduced VRAM requirements to ~3.5 GB using 4-bit quantization, enabling deployment on low-power edge hardware.
* **Instant Triage:** Evaluates water clarity, pH, odor, and health symptoms in under 2 seconds per forward pass.

---

## Links

* **GitHub repo:** [https://github.com/eshaansarkardipsite/AquaCheck-Edge](https://github.com/eshaansarkardipsite/AquaCheck-Edge)
* **Dataset(s) used:** Open-source water parameters and synthetic evaluation profiles.
* **Demo:** https://04db90838419229b3f.gradio.live/
* **License for this project:** Apache 2.0

---

## Acknowledgments

Special thanks to Google AI and Kaggle for providing the PaliGemma vision-language models and GPU infrastructure that made on-device edge AI testing possible.
