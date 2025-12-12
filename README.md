# 🦖 Dino TTS - Secure AI Voice Generator

**Dino TTS** is a secure, serverless Text-to-Speech (TTS) web application powered by **Google Gemini 2.5 Flash**.

Unlike typical client-side wrappers, this project implements a **Backend-for-Frontend (BFF)** pattern using **Cloudflare Workers**. This architecture ensures that sensitive API keys and proprietary system prompts are never exposed to the browser, making the application production-ready and secure against unauthorized usage.

🔗 **Live Demo:** [https://magicalgnome721.github.io/dino-tts/](https://magicalgnome721.github.io/dino-tts/)

---

## 🚀 Key Features

* **High-Fidelity AI Voices:** Generates realistic speech using Gemini's advanced multi-modal capabilities.
* **Secure Serverless Proxy:** Uses Cloudflare Workers to act as an intermediary, hiding API keys and enforcing strict CORS policies.
* **Hidden Prompt Engineering:** System instructions (e.g., "Discovery Channel" narration style) are injected server-side, preventing prompt leakage.
* **Smart Text Chunking:** Includes a custom algorithm to split long scripts (>2000 chars) into logical segments, ensuring stability and preventing API timeouts.
* **Load Balancing:** Implements logic to rotate through multiple API keys to handle rate limiting effectively.
* **Multi-Style Support:** Offers distinct narration styles including *Discovery (Documentary)*, *Storytelling*, and *War Mode*.

## 🛠️ Tech Stack

* **Frontend:** HTML5, Tailwind CSS, Vanilla JavaScript.
* **Backend / Edge:** Cloudflare Workers (Serverless).
* **AI Model:** Google Gemini 2.5 Flash (via REST API).
* **Hosting:** GitHub Pages (Frontend), Cloudflare Edge Network (Backend).

## 🏗️ System Architecture

To prevent API Key theft and usage quota abuse, this app does **not** make direct calls to Google from the client.

```mermaid
graph LR
    User["User / Browser"] -- "1. Send Text + Style" --> Worker["Cloudflare Worker (Secure Proxy)"]
    subgraph Secure Backend
        Worker -- "2. Auth Check (CORS)" --> Auth{"Valid Origin?"}
        Auth -- Yes --> Logic["3. Inject Hidden Prompt & Select API Key"]
        Logic -- "4. Request Audio" --> Gemini["Google Gemini API"]
    end
    Gemini -- "5. Return Audio Blob" --> Worker
    Worker -- "6. Return WAV to Client" --> User

Client: Sends only the raw text and selected style/voice to the Cloudflare Worker.

Worker:

Validates the request origin (must be magicalgnome721.github.io).

Selects an active API Key from encrypted environment variables.

Injects the proprietary system prompt (e.g., "Narrate in a calm, documentary tone...").

Forwards the request to Google's servers.

Google: Generates the audio and returns it to the Worker.

Worker: Relays the safe audio binary back to the user.

🛡️ Security Implementation
This project addresses common security pitfalls in frontend AI apps:

No Hardcoded Keys: Source code contains zero API credentials. Keys are stored in Cloudflare's Encrypted Environment Variables.

Origin Validation: The backend uses Access-Control-Allow-Origin to strictly allow requests only from the production domain, blocking local execution or unauthorized cloning.

Prompt Protection: The specific "Discovery" narration prompt is stored on the server, protecting the intellectual property of the prompt engineering.

⚙️ Local Development (For Reviewers)
Since the backend is deployed on Cloudflare, you can run the frontend locally, but it will only work if you update the WORKER_URL or set up your own worker.

Clone the repo:

Bash

git clone [https://github.com/MagicalGnome721/dino-tts.git](https://github.com/MagicalGnome721/dino-tts.git)
Open index.html in your browser.

Note: The production Worker will reject requests from localhost due to CORS policy. To test fully, please visit the Live Demo.

👨‍💻 Author
Nguyen An Khuong

Role: AI Engineer / Data Analyst

Focus: Building scalable AI solutions & Automation pipelines.

LinkedIn

GitHub Profile

Created as a portfolio project to demonstrate secure AI integration patterns.
