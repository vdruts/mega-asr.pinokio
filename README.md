# Mega-ASR — Pinokio Launcher

One-click installer for [Mega-ASR](https://github.com/xzf-thu/Mega-ASR), a foundation speech recognition model built for robust transcription in challenging real-world audio environments.

## What it does

Mega-ASR transcribes audio accurately even in noisy, far-field, echoing, or reverberant conditions. It was trained on 2.6M samples across 54 acoustic scenarios using a two-stage approach (supervised fine-tuning + reinforcement learning).

## How to use

1. Click **Install** in Pinokio — this clones the repo, installs dependencies, and downloads the model (~8GB VRAM recommended)
2. Click **Start** — launches the Gradio web UI
3. Upload an audio file or record from your microphone
4. Get your transcription

## Requirements

- GPU with 8GB+ VRAM (NVIDIA recommended, Apple Silicon supported)
- ~10GB disk space for model weights + dependencies

## API

### Python

```python
from transformers import AutoModelForCausalLM, AutoProcessor
import librosa

model = AutoModelForCausalLM.from_pretrained("zhifeixie/Mega-ASR", trust_remote_code=True)
processor = AutoProcessor.from_pretrained("zhifeixie/Mega-ASR", trust_remote_code=True)

audio, sr = librosa.load("audio.wav", sr=16000)
inputs = processor(audio=audio, sampling_rate=sr, return_tensors="pt")
outputs = model.generate(**inputs)
text = processor.batch_decode(outputs, skip_special_tokens=True)[0]
print(text)
```

### CLI

```bash
cd app
python infer.py --audio /path/to/audio.wav
```

### cURL (when web UI is running)

```bash
curl -X POST http://localhost:7860/api/predict \
  -H "Content-Type: application/json" \
  -d '{"data": ["audio.wav"]}'
```

## Credits

- **Model & Code:** [xzf-thu/Mega-ASR](https://github.com/xzf-thu/Mega-ASR)
- **Paper:** [arXiv](https://arxiv.org/abs/2505.xxxxx)
- **HuggingFace:** [zhifeixie/Mega-ASR](https://huggingface.co/zhifeixie/Mega-ASR)
