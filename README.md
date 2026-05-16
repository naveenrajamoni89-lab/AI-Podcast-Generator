
# AI Podcast & Audio Generator 🎙️🤖

An automated, text-to-speech n8n workflow that transforms a simple text prompt into a fully produced, high-quality audio podcast snippet. It leverages **Google Gemini** for localized, contextual script writing and connects directly with **Murf.ai's Next-Gen Falcon Engine** to stream high-fidelity voice output.

---

## 🚀 How It Works


```

┌───────────────────────────┐
│ When Chat Message Received│  <-- (User sends a topic)
└─────────────┬─────────────┘
▼
┌───────────────────────────┐
│      Basic LLM Chain      │  <-- (Generates custom 2-minute script)
└──────┬─────────────┬──────┘
│             ▲
▼             │
┌────────────────────┴──────┐
│ Google Gemini Chat Model  │
└───────────────────────────┘
▼
┌───────────────────────────┐
│    Murf.ai API Node       │  <-- (Streams real-time audio via Falcon)
└───────────────────────────┘

```

1. **Trigger:** The workflow starts via a chat interface web-hook trigger (`chatTrigger`), prompting the user to input any desired topic or theme.
2. **Script Generation:** The core text input is forwarded to a **Basic LLM Chain** backed by **Google Gemini**. A highly tuned system prompt directs the LLM to write a conversational, engaging, friendly, and informative 2-minute script formatted strictly as clean plain text.
3. **Audio Synthesis:** The generated script is packaged into an HTTP request payload and transmitted to **Murf.ai's Advanced Streaming Endpoint** (`/v1/speech/stream`).
4. **Voice Rendering:** The workflow utilizes Murf's ultra-realistic **Falcon Engine** using the voice profile `Alicia` with a `Conversation` tone setting to return a natural-sounding audio broadcast file.

---

## 🛠️ Tech Stack & Architecture

* **Workflow Engine:** [n8n](https://n8n.io/) (v1.0+)
* **Large Language Model:** Google Gemini AI (`lmChatGoogleGemini`)
* **Audio Synthesis Engine:** [Murf.ai API](https://murf.ai/) (Falcon Voice Generation Architecture)
* **Protocol Handling:** HTTP Post Node optimized for raw JSON streaming outputs

---

## 📁 Repository Contents

* `ai_audio_generator.json` - The entire n8n workflow canvas configuration mapping out all models, API paths, headers, and parameter definitions.
* `README.md` - Technical project specifications.

---

## ⚙️ Setup and Installation

### Prerequisites
1. A running instance of **n8n**.
2. A **Google AI Studio API Key** (for generating the text scripts).
3. A **Murf.ai Developer API Key** (for speech synthesis).

### Deployment Steps
1. Clone this repository or download the `ai_audio_generator.json` file.
2. Log into your n8n workspace dashboard.
3. Create a blank canvas, click the menu icon in the top right, and choose **Import from File**. Load your downloaded `.json` file.
4. **Configure Gemini Credentials:** Click on the *Google Gemini Chat Model* node and add your Google PaLM/Gemini API credentials.
5. **Configure Murf.ai Credentials:** Click on the *HTTP Request* node. Under headers, locate the `api-key` value parameter and replace it with your personal Murf.ai developer key.
6. Toggle the workflow to **Active** or press **Execute Workflow** to test the setup.

---

## 📋 API Implementation Notes

The workflow interacts directly with the Murf.ai streaming payload schema. It dynamically interpolates the LLM response data directly into the API body using the following configuration profile:

```json
{
  "text": "{{ $json.text }}",
  "voiceId": {
    "voice_id": "Alicia",
    "style": "Conversation",
    "model:": "Falcon"
  },
  "locale": "en-US",
  "model": "FALCON"
}

```

```

```
