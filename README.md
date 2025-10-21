# Jetson HA Voice Pipeline
 Running a local voice pipeline for Home Assistant on a Jetson Orin Nano

**work in progress**

This repository provides a Docker Compose setup for running a Home Assistant voice pipeline on NVIDIA Jetson devices. The stack includes services for speech-to-text, text-to-speech, LLMs, and development tools, all optimized for Jetson hardware. Home Assistant is supposed to run on another hardware and connect to these services through the Wyoming protocol or APIs, but can be run on Jetson too if the corresponding containers are enabled.

---

#### Files

- compose-core.yaml:
  Contains the core services Whisper, Piper and Ollama
- compose-development.yaml:
  Contains development tools (currentyl only Open WebUI)
- compose.yaml:
  Old version, only kept until all development tools are migrated to compose-development.yaml

---

#### Usage

- To start all core services, run:
  ```
  docker compose -f compose-core.yaml up
  ```
- To start development services, run:
  ```
  docker compose -f compose-development.yaml up
  ```
- To start a specific service, run:
  ```
  docker compose start piper-trainer
  ```

---

#### Services

**Main Services:**

- **faster-whisper**  
  Runs Whisper speech-to-text.  
  Based on container and compose.yaml from [dusty-nv - Jetson containers](https://github.com/dusty-nv/jetson-containers/tree/master/packages/smart-home/wyoming/wyoming-whisper).

- **piper-tts**  
  Text-to-speech using Piper.  
  Based on container and compose.yaml from [dusty-nv - Jetson containers](https://github.com/dusty-nv/jetson-containers/tree/master/packages/smart-home/wyoming/wyoming-piper).

- **ollama**  
  LLM inference (e.g., Llama 3.2, Phi-4-mini, Mistral, Qwen 3).
  Based on container from [dusty-nv - Jetson containers](https://github.com/dusty-nv/jetson-containers/tree/master/packages/llm/ollama).

**Development Services** (only with `--profile development`):

- **open-webui**  
  Web UI for interacting with LLMs via Ollama.  
  Based on container and compose.yaml from [open-webui](https://github.com/open-webui/open-webui).

- **piper-trainer**  
  TextyMcSpeechy training environment for Piper TTS models. The official notebook was not working for me because of broken dependencies. This tool is based on piper but greatly facilitates the training.
  Based on container and compose.yaml from [domesticatedviking - TextyMcSpeechy](https://github.com/domesticatedviking/TextyMcSpeechy).

- **microwakeword-trainer**  
  Jupyter-based training notebook for custom wake words. These wakewords can be used e.g. on the Home Assistant Voice Preview Edition. The Google Colab version was not working for me because of broken dependencies (Python version).
  Based on container from [MasterPhooey - MicroWakeWord-Trainer-Docker](https://github.com/MasterPhooey/MicroWakeWord-Trainer-Docker).

---

#### Notes

- The compose files are optimized for NVIDIA Jetson devices and uses the `nvidia` runtime.
- Some services (like Home Assistant, Assist Microphone, and OpenWakeWord) are present but commented out. Uncomment and configure as needed.

---

#### References

- [dusty-nv/jetson-containers](https://github.com/dusty-nv/jetson-containers)
- [open-webui/open-webui](https://github.com/open-webui/open-webui)
- [domesticatedviking/TextyMcSpeechy](https://github.com/domesticatedviking/TextyMcSpeechy)
- [MasterPhooey/MicroWakeWord-Trainer-Docker](https://github.com/MasterPhooey/MicroWakeWord-Trainer-Docker)

---

Feel free to customize the services and environment variables to fit your use case!
