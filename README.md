<h1 align="center">
</h1>
<p align="center">
  <p align="center"><b>End-to-end open-source voice agents platform</b>: Quickly build voice firsts conversational assistants through a json. </p>
</p>

<h4 align="center">
  <a href="https://discord.gg/59kQWGgnm8">Discord</a> |
  <a href="https://docs.bolna.ai">Hosted Docs</a> |
  <a href="https://bolna.ai">Website</a>
</h4>

<h4 align="center">
  <a href="https://discord.gg/59kQWGgnm8">
      <img src="https://img.shields.io/static/v1?label=Chat%20on&message=Discord&color=blue&logo=Discord&style=flat-square" alt="Discord">
  </a>
  <a href="https://github.com/bolna-ai/bolna/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="Bolna is released under the MIT license." />
  </a>
</h4>


## Introduction

**[Bolna](https://bolna.ai)** is the end-to-end open source production ready framework for quickly building LLM based voice driven conversational applications.

> [!NOTE]
> **New Integration**: AssemblyAI ASR provider has been added with real-time streaming support for enhanced transcription capabilities.

## Demo
https://github.com/bolna-ai/bolna/assets/1313096/2237f64f-1c5b-4723-b7e7-d11466e9b226


## What is this repository?
This repository contains the entire orchestration platform to build voice AI applications. It technically orchestrates voice conversations using combination of different ASR+LLM+TTS providers and models over websockets.


## Components
Bolna helps you create AI Voice Agents which can be instructed to do tasks through:

1. Orchestration platform (this open source repository)
2. Hosted APIs built on top of this orchestration platform
3. No-code UI playground using the hosted APIs

## Supported providers and models
1. Initiating a phone call using telephony providers like `Twilio`, `Plivo`, `Exotel` (coming soon), `Vonage` (coming soon) etc.
2. Transcribing the conversations using `Deepgram`, `AssemblyAI`, etc.
3. Using LLMs like `OpenAI`, `DeepSeek`, `Llama`, `Cohere`, `Mistral`,  etc to handle conversations
4. Synthesizing LLM responses back to telephony using `AWS Polly`, `ElevenLabs`, `Deepgram`, `OpenAI`, `Azure`, `Cartesia`, `Smallest` etc.

## Local Setup
A basic local setup includes usage of [Twilio](local_setup/telephony_server/twilio_api_server.py) or [Plivo](local_setup/telephony_server/plivo_api_server.py) for telephony. We have dockerized the setup in `local_setup/`. One will need to populate an environment `.env` file from `.env.sample`.

The setup consists of four containers:

1. Telephony web server (Twilio or Plivo)
2. Bolna server for creating and handling agents 
3. `ngrok` for tunneling
4. `redis` for persisting agents & prompt data

### Quick Start

The easiest way to get started is to use the provided script:

```bash
cd local_setup
chmod +x start.sh
./start.sh
```

This script will check for Docker dependencies, build all services with BuildKit enabled, and start them in detached mode.

### Manual Setup

Alternatively, you can manually build and run the services:

1. Make sure you have Docker with Docker Compose V2 installed
2. Enable BuildKit for faster builds:
   ```bash
   export DOCKER_BUILDKIT=1
   export COMPOSE_DOCKER_CLI_BUILD=1
   ```
3. Build the images:
   ```bash
   docker compose build
   ```
4. Run the services:
   ```bash
   docker compose up -d
   ```

To run specific services only:

```bash
docker compose up -d bolna-app twilio-app
# or
docker compose up -d bolna-app plivo-app
```

Once the docker containers are up, you can now start to create your agents and instruct them to initiate calls.

## Using your own providers
You can populate the `.env` file to use your own keys for providers.

<details>

<summary>ASR Providers</summary><br>
These are the current supported ASRs Providers:

| Provider     | Environment variable to be added in `.env` file |
|--------------|-------------------------------------------------|
| Deepgram     | `DEEPGRAM_AUTH_TOKEN`                           |
| AssemblyAI   | `ASSEMBLYAI_API_KEY`                            |

**AssemblyAI Features:**
- Real-time WebSocket streaming transcription
- Support for multiple languages (en, es, fr, etc.)
- Configurable models ('best', 'nova-2', etc.)
- Multi-telephony provider compatibility
- Both streaming and non-streaming modes

</details>
&nbsp;<br>

<details>
<summary>LLM Providers</summary><br>
Bolna uses LiteLLM package to support multiple LLM integrations.

These are the current supported LLM Provider Family:
https://github.com/bolna-ai/bolna/blob/10fa26e5985d342eedb5a8985642f12f1cf92a4b/bolna/providers.py#L30-L47

For LiteLLM based LLMs, add either of the following to the `.env` file depending on your use-case:<br><br>
`LITELLM_MODEL_API_KEY`: API Key of the LLM<br>
`LITELLM_MODEL_API_BASE`: URL of the hosted LLM<br>
`LITELLM_MODEL_API_VERSION`: API VERSION for LLMs like Azure

For LLMs hosted via VLLM, add the following to the `.env` file:<br>
`VLLM_SERVER_BASE_URL`: URL of the hosted LLM using VLLM

</details>
&nbsp;<br>

<details>

<summary>TTS Providers</summary><br>
These are the current supported TTS Providers:
https://github.com/bolna-ai/bolna/blob/c8a0d1428793d4df29133119e354bc2f85a7ca76/bolna/providers.py#L7-L14

| Provider   | Environment variable to be added in `.env` file  |
|------------|--------------------------------------------------|
| AWS Polly  | Accessed from system wide credentials via ~/.aws |
| Elevenlabs | `ELEVENLABS_API_KEY`                             |
| OpenAI     | `OPENAI_API_KEY`                                 |
| Deepgram   | `DEEPGRAM_AUTH_TOKEN`                            |
| Cartesia   | `CARTESIA_API_KEY`                            |
| Smallest   | `SMALLEST_API_KEY`                            |

</details>
&nbsp;<br>

<details>

<summary>Telephony Providers</summary><br>
These are the current supported Telephony Providers:

| Provider | Environment variable to be added in `.env` file                                                                                                                    |
|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Twilio   | `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_PHONE_NUMBER`|
| Plivo    | `PLIVO_AUTH_ID`, `PLIVO_AUTH_TOKEN`, `PLIVO_PHONE_NUMBER`|

</details>
