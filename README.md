Multimodal LLMOps Compliance Engine
AI-powered video advertisement auditing with multimodal analysis, retrieval-augmented generation (RAG), and workflow orchestration.

The Multimodal LLMOps Compliance Engine is a prototype system that helps review video advertisements for potential brand-safety and regulatory compliance risks. It brings together spoken content, on-screen text, video metadata, and relevant policy documents to produce context-aware compliance assessments.
Project status: Prototype. AI-generated compliance findings are decision-support outputs, not legal advice or a substitute for human review.

Overview
Manually reviewing video advertisements against multiple brand guidelines and regulatory policies can be slow and inconsistent. This project explores a more systematic approach: extract signals from the video, retrieve relevant rules, and use an LLM to assess potential issues with supporting context.
Key Features
- Video ingestion: Process YouTube video inputs through Azure Video Indexer.
- Multimodal extraction: Collect speech transcripts, on-screen text via OCR, and video metadata.
- Policy-aware auditing: Chunk PDF policy documents and retrieve relevant passages with Azure AI Search and Azure OpenAI embeddings.
- LLM-powered assessment: Use Azure OpenAI (GPT-4.1-mini) to analyze content against retrieved guidance.
- Workflow orchestration: Coordinate video indexing and compliance analysis through a sequential LangGraph workflow.
- API integration: Expose the system through a FastAPI application.
- Observability: Instrument FastAPI requests and application telemetry using Azure Monitor OpenTelemetry.
Architecture
YouTube Video
     |
     v
Azure Video Indexer
     |-- Speech transcript
     |-- On-screen OCR text
     `-- Video metadata
     |
     v
LangGraph Audit Workflow <----- Azure AI Search
     |                             ^
     |                             |
     |                     PDF Policy Documents
     |                       -> Chunking
     |                       -> Embeddings
     |                       -> Search Index
     v
Azure OpenAI (GPT-4.1-mini)
     |
     v
Compliance Assessment
     |
     v
FastAPI Response

Azure Monitor OpenTelemetry -> API / application telemetry
Tech Stack
Area	Technologies
Language	Python 3.12+
API	FastAPI
Orchestration	LangGraph
LLM	Azure OpenAI (GPT-4.1-mini)
Video processing	Azure Video Indexer
Retrieval / RAG	Azure AI Search, Azure OpenAI embeddings
Policy sources	PDF documents
Monitoring	Azure Monitor OpenTelemetry
Environment / dependencies	.env, uv or pip


Getting Started
Prerequisites
- Python 3.12 or newer
- Access to the Azure services used by your configured workflow
- Appropriate Azure and other service credentials
- uv (optional) or pip
1. Clone the repository
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_DIRECTORY>
2. Set up the Python environment
Option A — using uv:
uv sync
Option B — using pip:
python -m venv .venv
Activate the environment:
# macOS / Linux
source .venv/bin/activate

# Windows (PowerShell)
.venv\Scripts\Activate.ps1
Then install the project:
python -m pip install -e .
3. Configure environment variables
Create a .env file in the project root and add the settings required by the application's configuration. These may include your Azure OpenAI, Azure Video Indexer, Azure AI Search, and monitoring credentials/endpoints.
Environment variable names and required values depend on the project's configuration code. Refer to that configuration before running the application; no sample variable names are assumed here.

Security: .env is intentionally ignored by Git. Never commit API keys, tokens, connection strings, or other secrets.
4. Run the application
Start the application using the entry point configured for the workflow you want to run (for example, the FastAPI service or the project's audit workflow). Consult the repository's application entry point for the exact command.
How the Audit Works
1. Ingest video: Submit a supported video input for indexing.
2. Extract content: Gather the transcript, OCR text, and available metadata.
3. Retrieve policies: Search indexed PDF policy passages for relevant requirements.
4. Analyze compliance: Pass the extracted signals and retrieved rules to the LLM through the LangGraph workflow.
5. Return findings: Produce a compliance assessment for further review.
6. Observe execution: Capture configured API and application telemetry to support troubleshooting.
Scope and Limitations
- This is a prototype and should not be treated as an autonomous compliance certification system.
- Findings depend on source-video quality, extracted transcript/OCR accuracy, policy coverage, retrieval relevance, and model behavior.
- A human reviewer should validate important decisions, especially regulatory or legal conclusions.
- Service availability, authentication, usage limits, and cloud costs depend on your Azure configuration.
Security Best Practices
- Keep credentials in local environment variables or a suitable secret manager.
- Restrict access to cloud resources using least-privilege permissions.
- Avoid logging secrets or sensitive video/policy contents.
- Review generated findings before using them in production decisions.
Future Improvements
- Add documented request/response examples and an API reference.
- Add automated tests for ingestion, retrieval, and audit workflows.
- Introduce evaluation datasets to measure retrieval and assessment quality.
- Improve policy traceability and reviewer feedback workflows.
Purpose: Demonstrate how multimodal AI, RAG, cloud services, and LLMOps practices can support scalable video compliance review.
