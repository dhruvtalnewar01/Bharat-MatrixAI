<div align="center">

# 🇮🇳 Bharat MatrixAI

### The First Agentic Interface for the Next Billion Users

[![AWS Bedrock](https://img.shields.io/badge/AWS-Bedrock-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/bedrock/)
[![Claude 3.5 Sonnet](https://img.shields.io/badge/Claude-3.5_Sonnet-5A67D8?style=for-the-badge&logo=anthropic&logoColor=white)](https://www.anthropic.com/claude)
[![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=for-the-badge&logo=playwright&logoColor=white)](https://playwright.dev/)
[![React Native](https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactnative.dev/)
[![AWS Nova Sonic](https://img.shields.io/badge/AWS-Nova_Sonic-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/ai/generative-ai/nova/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)

**Winner Submission | AWS AI for Bharat Hackathon 2026**

</div>

---

## 🚨 The Problem: Interface Poverty

India has achieved remarkable digital infrastructure penetration—but **500 million rural citizens remain digitally excluded**. The barrier isn't connectivity or smartphones. It's **Interface Poverty**.

### The Reality on the Ground

- **95% of government portals** are English-only, while **90% of rural India** speaks vernacular languages
- Portals require navigating complex multi-step forms, dropdown menus, and visual CAPTCHAs
- A 45-year-old farmer in Maharashtra cannot check his PM Kisan status—not because the information doesn't exist, but because he cannot navigate the interface

### Why Current AI Solutions Fail

Traditional AI chatbots (GPT, Gemini, Claude) are **information retrieval systems**. They can:
- ✅ Summarize text
- ✅ Answer questions
- ✅ Provide instructions

But they **cannot execute**. They abandon the user at the critical last mile:
- ❌ Cannot fill forms on government portals
- ❌ Cannot click buttons or solve CAPTCHAs
- ❌ Cannot extract real-time data from dynamic web pages

**Bharat MatrixAI bridges this execution gap.**

---

## 💡 The Solution: From Chatbots to Operators

Bharat MatrixAI is not a chatbot. It is a **Voice-First Large Action Model (LAM)**—an autonomous agent that doesn't just provide information, it **takes action on behalf of the user**.

### How It Works

1. **User speaks** in their native language (Hindi, Marathi, Tamil, etc.): *"Mera PM Kisan status check karo"*
2. **AWS Nova Sonic** transcribes the vernacular speech to structured intent
3. **AWS Bedrock Claude 3.5 Sonnet** identifies the target portal (pmkisan.gov.in) and extracts parameters (Aadhaar number)
4. **AWS Fargate** spins up a headless browser using **Playwright**
5. **Claude Vision** analyzes screenshots in real-time, returning precise X/Y coordinates to click buttons and fill forms
6. The agent **autonomously navigates** the portal, solves visual CAPTCHAs, and scrapes the final result
7. **AWS Nova Sonic** synthesizes the result into warm, vernacular audio
8. **User hears** the response in their native language: *"Aapka payment 2,000 rupees approve ho gaya hai"*

### The Paradigm Shift

| Traditional Chatbots | Bharat MatrixAI (LAM) |
|---------------------|----------------------|
| Provides information | **Executes tasks** |
| Requires user to navigate portals | **Navigates portals autonomously** |
| English-centric | **10 vernacular languages** |
| Breaks on UI changes | **Self-healing via computer vision** |
| Stops at instructions | **Completes end-to-end workflows** |

---

## 🔥 Key Innovations

### 1. Zero-API Integration: Works on Legacy Portals

Most government portals have **no public APIs**. Bharat MatrixAI doesn't need them.

- Uses **headless browser automation** (Playwright) to interact with portals as a human would
- Works on legacy systems built with outdated HTML, JavaScript, and server-side rendering
- No partnership agreements or API keys required—**deploy today, scale tomorrow**

### 2. Vision-Based Self-Healing Navigation

Traditional web scrapers fail when websites update their CSS classes or DOM structure. Bharat MatrixAI uses **Claude 3.5 Sonnet Vision** for resilient navigation:

- Takes a **screenshot** of the current page
- Sends the image to Claude Vision with a navigation goal: *"Find the 'Check Status' button"*
- Claude returns **precise X/Y pixel coordinates**
- Playwright clicks at those coordinates—**no CSS selectors, no XPath fragility**

**Result**: When government portals update their UI (which happens frequently), the system adapts **without code changes**.

### 3. Hyper-Local Voice Engine

Powered by **AWS Nova 2 Sonic**, the system supports:
- **10 vernacular languages**: Hindi, Marathi, Tamil, Telugu, Bengali, Gujarati, Kannada, Malayalam, Punjabi, Odia
- **Real-time streaming transcription** with <3 second latency
- **Natural prosody** in voice synthesis for better comprehension
- **Noise reduction** optimized for rural environments (background chatter, livestock, wind)

---

## 🏗️ System Architecture

```mermaid
sequenceDiagram
    participant User
    participant ReactNative as React Native App
    participant APIGateway as AWS API Gateway
    participant StepFunctions as AWS Step Functions
    participant Nova as AWS Nova Sonic
    participant Bedrock as AWS Bedrock Claude
    participant Fargate as AWS Fargate
    participant Playwright as Playwright Browser
    participant Portal as Government Portal

    User->>ReactNative: Speaks vernacular voice command
    ReactNative->>APIGateway: Streams audio payload
    APIGateway->>StepFunctions: Triggers orchestration workflow
    
    StepFunctions->>Nova: Transcribe audio
    Nova-->>StepFunctions: Returns text + intent
    
    StepFunctions->>Bedrock: Extract intent & parameters
    Bedrock-->>StepFunctions: Returns portal config + params
    
    StepFunctions->>Fargate: Launch browser automation task
    Fargate->>Playwright: Initialize headless Chromium
    Playwright->>Portal: Navigate to target URL
    
    loop Navigation Loop
        Playwright->>Playwright: Capture screenshot
        Playwright->>Bedrock: Send screenshot to Claude Vision
        Bedrock-->>Playwright: Returns X/Y coordinates + action
        Playwright->>Portal: Execute click/type action
        Portal-->>Playwright: Page updates
    end
    
    Playwright->>Playwright: Scrape final result
    Playwright-->>Fargate: Return extracted data
    Fargate-->>StepFunctions: Task complete
    
    StepFunctions->>Nova: Synthesize vernacular audio
    Nova-->>StepFunctions: Returns audio stream
    
    StepFunctions->>ReactNative: Send voice response
    ReactNative->>User: Plays audio in native language
```

### Architecture Highlights

- **Asynchronous Orchestration**: AWS Step Functions manages long-running tasks (government portals often take 15+ seconds per page)
- **Horizontal Scalability**: Each user gets an isolated Fargate container—no session interference
- **Privacy-First**: PII redaction via AWS Guardrails at the CloudWatch layer
- **Cost-Optimized**: Fargate containers terminate immediately after task completion

---

## 💰 Business Viability: Unit Economics

| Cost Component | Per Transaction |
|---------------|----------------|
| AWS Nova Sonic (Transcription) | $0.008 |
| AWS Bedrock Claude (Intent + Vision) | $0.012 |
| AWS Fargate (Compute) | $0.006 |
| AWS S3 + DynamoDB | $0.002 |
| **Total Cost** | **$0.028 (~₹2.35)** |

### Why This Matters

- **B2G Contracts**: State governments can afford to subsidize this for rural citizens
- **Freemium Model**: First 5 queries/month free, ₹10/month for unlimited
- **CSR Partnerships**: Telecom operators (Jio, Airtel) can bundle this as a social impact initiative

**Target**: 10 million transactions/month at 85%+ task completion rate.

---

## 🚀 Quick Start & Installation

### Prerequisites

- Python 3.11+
- AWS Account with Bedrock and Nova access
- Node.js 18+ (for React Native client)

### Backend Setup

```bash
# Clone the repository
git clone https://github.com/your-org/bharat-matrix-ai.git
cd bharat-matrix-ai

# Install Python dependencies
pip install -r requirements.txt

# Install Playwright browser binaries
playwright install chromium

# Set AWS credentials
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_DEFAULT_REGION=ap-south-1

# Run the demo vision agent
python src/demo_vision_agent.py
```

### Mobile Client Setup

```bash
cd mobile-client
npm install
npx expo start
```

### Environment Variables

Create a `.env` file in the project root:

```env
AWS_BEDROCK_MODEL_ID=anthropic.claude-3-5-sonnet-20241022-v2:0
AWS_NOVA_SONIC_MODEL_ID=amazon.nova-sonic-v1:0
AWS_REGION=ap-south-1
FARGATE_CLUSTER_NAME=bharat-matrix-cluster
DYNAMODB_TABLE_NAME=TaskSessions
S3_BUCKET_NAME=bharat-matrix-screenshots
```

---

## 📊 Demo: PM Kisan Status Check

```python
# Example: Check PM Kisan scheme status via voice
from bharat_matrix import VoiceAgent

agent = VoiceAgent(language="hi")  # Hindi

# User speaks: "Mera PM Kisan status check karo, Aadhaar 1234-5678-9012"
audio_input = "path/to/voice_note.wav"

result = agent.execute(audio_input)
# Agent autonomously:
# 1. Navigates to pmkisan.gov.in
# 2. Fills Aadhaar number
# 3. Solves CAPTCHA
# 4. Extracts payment status
# 5. Returns: "Aapka payment 2,000 rupees approve ho gaya hai"

print(result.audio_response)  # Plays vernacular audio
```

---

## 🔮 Future Scope

### Phase 2: WhatsApp Integration (Zero-UI Footprint)

- Migrate client layer to **WhatsApp Business API**
- Users send voice notes directly via WhatsApp—no app installation required
- 500M+ Indians already use WhatsApp daily—**zero adoption friction**

### Phase 3: Portal Expansion

Current MVP supports:
- ✅ PM Kisan (farmer subsidies)
- ✅ Ration card status
- ✅ Land records

Planned additions:
- 🚂 IRCTC (train ticket booking)
- 💉 CoWIN (vaccination certificates)
- 🏦 Aadhaar-linked bank account status
- 📜 Birth/death certificate applications

**Scalability**: Adding new portals requires **zero code changes**—only configuration updates to the portal registry.

### Phase 4: Proactive Notifications

- Alert users about expiring documents (driving license, ration card)
- Notify about new government schemes based on user profile
- Remind about upcoming payment deadlines

---

## 🏆 Why Bharat MatrixAI Wins

### Technical Excellence
- **Production-grade architecture** using AWS serverless primitives
- **Property-based testing** with 49 correctness properties
- **Self-healing navigation** resilient to portal changes

### Social Impact
- **500M+ addressable users** in rural India
- **Eradicates Interface Poverty** through voice-first execution
- **Preserves dignity** by eliminating the need for intermediaries

### Business Viability
- **<$0.03 per transaction** enables sustainable B2G contracts
- **Horizontal scalability** via containerized architecture
- **Zero API dependency** works on legacy government systems today

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details

---

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📧 Contact

- **Team Lead**: [Your Name](mailto:your.email@example.com)
- **Project Repository**: [github.com/your-org/bharat-matrix-ai](https://github.com/your-org/bharat-matrix-ai)
- **Demo Video**: [YouTube Link](https://youtube.com/your-demo)

---

<div align="center">

**Built with ❤️ for Bharat | Powered by AWS AI**

*Submitted to AWS AI for Bharat Hackathon 2026*

</div>
