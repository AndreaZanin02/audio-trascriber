# Faster-Whisper Audio Transcriber (Google Colab)

A universal, and production-ready audio/video transcription tool optimized for the free tier of **Google Colab** (T4 GPU), powered by `faster-whisper` and Silero VAD (Voice Activity Detection).

---

### Quick Start

Run the notebook directly in your browser with zero local setup:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AndreaZanin02/audio-trascriber/blob/main/audio_trascriber.ipynb)


---

### Key Features

* **High Performance Inference:** Built on top of `faster-whisper` (CTranslate2) with GPU `float16` batched processing-delivering **4x to 8x faster** transcription compared to standard PyTorch implementations.
* **Anti-Hallucination & No Repetition Loops:** Integrated Silero VAD pipeline trims background silence, acoustic noise, and breathing sounds before decoding, eliminating common Whisper repetition bugs.
* **Dynamic Gain Normalization:** Pre-processes the audio signal via FFmpeg (`highpass` + `dynaudnorm`) to balance voices recorded at different distances from the microphone.
* **Universal Media Support:** Natively supports `.mp3`, `.wav`, `.m4a`, `.aac`, `.flac`, `.ogg`, `.opus`, `.wma`, as well as video containers (`.mp4`, `.mkv`, `.mov`, `.webm`, `.avi`).
* **Direct Local Download:** Automatically triggers a download of the resulting `.txt` transcription to your local machine.
* **Clean UI via Colab Forms:** Tweak language settings, model checkpoints (`large-v3-turbo`, `large-v3`, `medium`), timestamps, and domain-specific keywords via straightforward form inputs without modifying code.

---

### How to Use

1. **Verify GPU acceleration:**
   * In the top Colab menu, go to **Runtime** > **Change runtime type**.
   * Under *Hardware accelerator*, ensure **T4 GPU** is selected.

2. **Step 1: upload your file (Cell 1):**
   * Run **Cell 1**.
   * Click `Choose Files` to select an audio or video file from your computer.
   * *Tip for large files (>50 MB):* The Colab upload button uses chunked Base64 websockets, which can be slow. You can click `Cancel upload`, open the **Files** pane on the left sidebar (📁 icon), and drag-and-drop your file directly. The cell will detect it automatically.

3. **Step 2: transcribe and export (Cell 2):**
   * Choose your preferred target language (or keep `Auto-rilevamento` / Auto-detect).
   * Toggle timestamps on/off depending on your needs.
   * *(Optional)* If your audio contains specialized technical jargon, brand names, or proper nouns, enter them as comma-separated values in the optional keyword field (e.g. `MOC, Modbus RTU, Kubernetes, 5ms`). Leave it empty for general recordings.
   * Run the cell. The transcribed segments will stream in real time. Once finished, the `.txt` file will automatically download to your browser's default download directory.

---

###  Architecture and Under the hood

```
[Input audio/video]
        │
        ▼
[FFmpeg: 16kHz mono PCM WAV + highpass (80Hz) + dynamic audio normalizer]
        │
        ▼
[Silero VAD (voice activity detection) - filter non-speech chunks]
        │
        ▼
[faster-whisper (CTranslate2 float16 batched inference on T4 GPU)]
        │
        ▼
[Streaming output and local .txt browser download]
```

---

### License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
