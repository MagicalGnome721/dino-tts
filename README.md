🦖 Dino TTS - Secure AI Voice Generator

Dino TTS is a secure, serverless Text-to-Speech (TTS) web application powered by Google Gemini 2.5 Flash.

Unlike typical client-side wrappers, this project implements a Backend-for-Frontend (BFF) pattern using Cloudflare Workers. This architecture ensures that sensitive API keys and proprietary system prompts are never exposed to the browser, making the application production-ready and secure against unauthorized usage.

🔗 Live Demo: https://magicalgnome721.github.io/dino-tts/

🚀 Key Features

High-Fidelity AI Voices: Generates realistic speech using Gemini's advanced multi-modal capabilities.

Secure Serverless Proxy: Uses Cloudflare Workers to act as an intermediary, hiding API keys and enforcing strict CORS policies.

Hidden Prompt Engineering: System instructions (e.g., "Discovery Channel" narration style) are injected server-side, preventing prompt leakage.

Smart Text Chunking: Includes a custom algorithm to split long scripts (>2000 chars) into logical segments.

Load Balancing: Rotates through multiple API keys to handle quota.

Multi-Style Support: Documentary, Storytelling, War Mode.

🛠️ Tech Stack

Frontend: HTML5, Tailwind, Vanilla JS

Backend: Cloudflare Workers

Model: Gemini 2.5 Flash

Hosting: GitHub Pages + Cloudflare Edge

🏗️ System Architecture

To prevent API Key theft and usage quota abuse, this app does not make direct calls to Google from the client.

graph LR
    %% User sends text request
    User["🧑‍💻 User / Browser"] -- "1️⃣ Send Text + Style" --> Worker["🛡️ Cloudflare Worker (Secure Proxy)"]

    %% Secure backend logic
    subgraph Secure Backend 🔒
        Worker -- "2️⃣ Auth Check (CORS)" --> Auth{"Valid Origin?"}
        Auth -- "✔️ Yes" --> Logic["3️⃣ Inject Hidden Prompt & Select API Key"]
        Auth -- "❌ No" --> Block["Reject Request 🚫"]
        Logic -- "4️⃣ Request Audio" --> Gemini["🤖 Google Gemini API"]
    end

    %% Return path
    Gemini -- "5️⃣ Return Audio Blob" --> Worker
    Worker -- "6️⃣ Return WAV to Client" --> User

🧩 Flow Explanation (Plain English)

Client:
Sends raw text + selected voice/style → Worker.

Worker:
• Validates origin (magicalgnome721.github.io)
• Selects an API key from encrypted vars
• Injects the hidden narration prompt
• Sends request to Google

Google:
Returns audio binary.

Worker:
Returns WAV back to client — safe, no key exposure.

🛡️ Security Implementation

No Hardcoded Keys:
Keys stored in Cloudflare Encrypted Vars.

Strict CORS Validation:
Only the production domain can access the Worker.

Protected Prompt Engineering:
Narration instructions never leave the server.

⚙️ Local Development

You can run the frontend locally, but the Worker will reject localhost unless you modify CORS.

Clone the repo
git clone https://github.com/MagicalGnome721/dino-tts.git


Open index.html in your browser.

Full functionality requires your own Worker endpoint.

👨‍💻 Author

Nguyen An Khuong
AI Engineer / Data Analyst
Building scalable AI + automation systems.

LinkedIn
GitHub
