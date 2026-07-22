# Epic: Build a Secure Azure AI Legal Knowledge and Meeting Intelligence Platform

**Issue Type:** Epic  
**Priority:** High  
**Suggested Labels:** `azure`, `artificial-intelligence`, `legal-ai`, `rag`, `semantic-kernel`, `speech-to-text`, `terraform`, `learning-project`

## Summary

Design and build a secure, production-style Azure AI platform that demonstrates the skills required for an AI Solutions Specialist working in a legal organization.

The platform will provide:

- A legal document question-answering assistant
- Retrieval-Augmented Generation with verifiable source citations
- Audio transcription and structured summarization
- User feedback collection
- AI quality evaluations
- Microsoft 365 and SharePoint integration
- Secure Azure deployment
- Monitoring, governance, documentation, and adoption reporting

The project should be implemented incrementally so each Azure service and AI concept can be learned, tested, documented, and demonstrated independently.

## Background

This repository will be used as an end-to-end Azure learning project and interview portfolio for an AI Solutions Specialist role in a legal environment.

The implementation should demonstrate practical experience with:

- Microsoft Azure
- Microsoft Foundry and Azure OpenAI
- Semantic Kernel
- Azure AI Search
- Azure Speech
- Microsoft Graph
- SharePoint
- Microsoft Entra ID
- Power Platform and Copilot Studio
- Terraform
- GitHub Actions
- AI evaluation, security, governance, and adoption measurement

## User Story

**As an AI Solutions Specialist,**  
I want to build and deploy a secure legal AI platform using Azure and Microsoft technologies,  
so that legal professionals can search approved documents, summarize meetings and recordings, receive grounded AI responses, provide feedback, and safely use AI within a governed enterprise environment.

## Goals

1. Learn the major Azure services used to build enterprise AI applications.
2. Build a working legal RAG assistant with citations and abstention behavior.
3. Add audio-to-text transcription and AI summarization.
4. Integrate the platform with SharePoint and Microsoft Graph.
5. Implement enterprise authentication, authorization, and matter-level access controls.
6. Measure groundedness, hallucinations, citation quality, latency, cost, and user satisfaction.
7. Deploy the platform using Terraform and GitHub Actions.
8. Add monitoring, governance, user documentation, and training material.
9. Produce a polished portfolio demonstration relevant to a legal AI role.

---

# Functional Requirements

## 1. Azure Foundation

Create the foundational Azure environment required by the application.

The environment must include:

- Azure Resource Group
- Azure Storage Account
- Azure Blob Storage containers
- Azure Key Vault
- Azure SQL or Cosmos DB
- Azure AI Search
- Microsoft Foundry or Azure OpenAI model deployment
- Azure Speech resource
- Application Insights
- Log Analytics workspace
- App Service, Container Apps, or Azure Functions
- Microsoft Entra ID application registration

Resources must use consistent naming, environments, tags, role assignments, secrets management, and cost controls.

Suggested environments:

```text
dev
test
prod
```

## 2. Azure-Hosted AI Chat Application

Build a web application that allows authenticated users to communicate with an Azure-hosted AI model.

The chatbot must support:

- System and user prompts
- Streaming responses
- Conversation history
- User session tracking
- Prompt version tracking
- Model version tracking
- Token usage tracking
- Response latency tracking
- Structured error handling
- Safe fallback responses
- "I do not have enough evidence" behavior
- Clear separation between frontend and backend

Suggested implementation options:

- Python with FastAPI
- TypeScript with Node.js
- Semantic Kernel
- Azure OpenAI or Microsoft Foundry

## 3. Semantic Kernel Agent

Use Semantic Kernel to build an AI agent capable of calling controlled application tools.

The agent should include plugins or functions for:

- Searching legal documents
- Retrieving source excerpts
- Summarizing documents
- Transcribing audio
- Summarizing transcripts
- Saving user feedback
- Loading conversation history
- Retrieving matter or case information
- Extracting action items and deadlines
- Recording tool-call audit information

Document:

- Why Semantic Kernel was selected
- How plugins are registered
- How tool calls are authorized
- How unsafe or unauthorized calls are rejected
- When Semantic Kernel should be used instead of LangChain or direct model calls

## 4. Legal Document RAG

Implement a Retrieval-Augmented Generation pipeline for legal documents.

Supported inputs should include:

- PDF documents
- Microsoft Word documents
- Plain-text files
- SharePoint documents

Each document should support metadata such as:

- Matter or case ID
- Document type
- Author
- Source system
- Upload date
- Confidentiality classification
- Authorized users or groups

### Ingestion Flow

1. Upload documents to Azure Blob Storage.
2. Extract document text and metadata.
3. Split documents into chunks.
4. Generate embeddings.
5. Store text, vectors, and metadata in Azure AI Search.
6. Track ingestion status and failures.
7. Make content searchable only by authorized users.

### Retrieval Flow

Support:

- Keyword search
- Vector search
- Hybrid search
- Semantic ranking
- Metadata filtering
- Matter-level filtering
- Configurable Top-K
- Citation generation
- Retrieval score tracking

The assistant must not answer from general model knowledge when the user requests an answer grounded in approved legal documents.

## 5. Source Citations and Grounding

Every document-grounded answer must include:

- Document name
- Page number or section when available
- Relevant source excerpt
- Matter or case identifier
- Citation link or document reference
- Retrieval relevance score for debugging

The system must:

- Avoid fabricated citations
- Verify that citations exist in retrieved context
- Abstain when evidence is insufficient
- Clearly distinguish sourced facts from AI-generated interpretation
- Prevent users from accessing documents outside authorized matters
- Prevent cross-matter information leakage

## 6. Audio-to-Text and Summarization

Build an audio processing workflow for legal meetings, interviews, depositions, and internal discussions.

### Workflow

1. Allow users to upload an audio file.
2. Store the original audio in Azure Blob Storage.
3. Submit the file to Azure Speech-to-Text.
4. Store the raw transcription.
5. Preserve timestamps when available.
6. Identify speakers when diarization is supported.
7. Send the transcript to Azure OpenAI or Foundry for summarization.
8. Save the transcript and generated summaries.
9. Allow users to review and rate the result.

### Generated Outputs

Support:

- Executive summary
- Detailed summary
- Meeting notes
- Deposition summary
- Action items
- Deadlines
- Questions requiring follow-up
- People and organizations mentioned
- Risks and concerns
- Legal issues identified
- Key events
- Chronological timeline
- Speaker-specific statements
- Timestamp citations

The user should be able to select a summary style.

## 7. Database and Data Model

Create a database design that supports the application.

Suggested entities:

```text
Users
Roles
Matters
UserMatterAccess
ChatSessions
Messages
Prompts
PromptVersions
Documents
DocumentChunks
AudioFiles
Transcripts
Summaries
Feedback
EvaluationRuns
EvaluationResults
ToolCalls
AuditLogs
UsageMetrics
```

Each AI response should store:

- User ID
- Session ID
- Prompt
- Prompt version
- Model and model version
- Generated response
- Retrieved sources
- Tool calls
- Input and output token counts
- Estimated cost
- Response latency
- Feedback status
- Creation timestamp

Sensitive data must not be written into operational logs unnecessarily.

## 8. User Feedback System

Allow users to evaluate AI responses and summaries.

Feedback options must include:

- Thumbs up
- Thumbs down
- Optional written comment
- Incorrect answer
- Missing information
- Incorrect citation
- Hallucination
- Inappropriate response
- Summary was inaccurate
- Summary missed an important detail

Store:

- User
- Response or summary ID
- Rating
- Comment
- Feedback category
- Model
- Prompt version
- Retrieved sources
- Timestamp

Provide an administrator view for reviewing negative feedback and identifying recurring failures.

## 9. Microsoft Entra ID and Access Control

Use Microsoft Entra ID for authentication and authorization.

Support:

- User authentication
- Application roles
- Administrator role
- Standard user role
- Matter-specific access
- Deny-by-default behavior
- Session expiration
- Managed identity where supported
- Authorization audit logs

Prevent:

- Anonymous access to confidential content
- Cross-matter document retrieval
- Unauthorized administrative actions
- Prompt-based bypass of permissions
- Direct access to protected storage objects

## 10. Microsoft Graph and SharePoint

Integrate the platform with Microsoft 365.

Demonstrate:

- Microsoft Graph authentication
- SharePoint site discovery
- SharePoint document library access
- OneDrive file access
- Document ingestion from SharePoint
- Source metadata preservation
- Permission-aware retrieval
- Refreshing documents when source content changes
- Linking citations back to SharePoint

Where practical, reuse Microsoft 365 permissions instead of duplicating them.

## 11. Copilot Studio and Power Platform

Build a small Copilot Studio proof of concept using the same legal knowledge source.

Include:

- SharePoint knowledge source
- At least one custom action
- At least one Power Automate workflow
- Teams or email notification
- A comparison between Copilot Studio and the custom Azure application

Document when to use:

- Copilot Studio
- Semantic Kernel
- Power Automate
- Custom Azure applications
- Direct Azure OpenAI integration

Optionally use Power BI for adoption and quality reporting.

## 12. Security and AI Governance

Implement and document enterprise security controls.

### Platform Controls

- Azure Key Vault
- Managed identities
- Role-Based Access Control
- Least-privilege permissions
- Encryption in transit
- Encryption at rest
- Secure configuration management
- Data retention policies
- Soft deletion
- Audit logs
- Secret rotation guidance
- Restricted network access where practical

### AI-Specific Controls

- Prompt-injection testing
- Jailbreak testing
- PII detection
- PII redaction
- Citation verification
- Hallucination detection
- Unsupported-answer rejection
- Input validation
- Output validation
- Tool-call authorization
- Confidential-data handling
- Prompt and model version tracking

Create an AI risk register containing:

```text
Risk
Impact
Likelihood
Mitigation
Owner
Testing method
Current status
```

## 13. AI Evaluation Framework

Create a reusable evaluation dataset and automated evaluation process.

Measure:

- Groundedness
- Answer correctness
- Citation correctness
- Retrieval relevance
- Completeness
- Hallucination rate
- Abstention quality
- Prompt-injection resistance
- PII leakage
- Response consistency
- Summary accuracy
- Action-item accuracy
- Latency
- Token usage
- Cost
- User satisfaction

The evaluation dataset should contain:

- Test question
- Expected answer
- Approved source documents
- Expected citations
- Expected behavior
- Risk level
- Evaluation category

Store enough metadata to compare:

- Prompt versions
- Model versions
- Embedding models
- Chunk sizes
- Chunk overlap
- Top-K values
- Search strategies
- Semantic ranking configuration

## 14. Monitoring and Observability

Use Application Insights and Azure Monitor.

Track:

- API request count
- Failed requests
- Authentication failures
- Model-call failures
- Search failures
- Average model latency
- End-to-end response latency
- Input and output tokens
- Estimated model cost
- Retrieval score
- Low-confidence responses
- User feedback
- Audio transcription failures
- Average transcription time
- Summary-generation failures
- Tool-call failures

Create alerts for:

- High error rate
- Authentication failures
- Model quota errors
- Excessive latency
- Failed document ingestion
- Failed transcription
- Unexpected cost increases

## 15. Infrastructure as Code

Use Terraform to define the Azure infrastructure.

Terraform must support:

- Repeatable deployment
- Environment-specific configuration
- Resource naming and tagging
- Remote state guidance
- Secret references
- Output values
- Safe destruction of test resources
- Deployment documentation

Credentials and secrets must not be committed to GitHub.

## 16. CI/CD

Create GitHub Actions workflows for:

- Dependency installation
- Formatting checks
- Linting
- Type checking
- Unit tests
- Security checks
- Terraform validation
- Terraform formatting checks
- Application build
- Deployment to development
- Post-deployment smoke tests

Production deployment should require manual approval.

## 17. Documentation and Training

Create:

```text
README.md
docs/architecture.md
docs/azure-setup.md
docs/local-development.md
docs/deployment.md
docs/security.md
docs/ai-governance.md
docs/rag-design.md
docs/audio-transcription.md
docs/sharepoint-integration.md
docs/evaluation.md
docs/troubleshooting.md
docs/user-guide.md
docs/admin-guide.md
docs/demo-script.md
docs/learning-log.md
```

The learning log should record:

- Azure service learned
- Purpose of the service
- What was implemented
- Problems encountered
- How each problem was fixed
- Security considerations
- Cost considerations
- Interview explanation
- Remaining learning gaps

Create an architecture diagram showing the complete data flow.

---

# Acceptance Criteria

The Epic is complete when:

1. The application can be deployed to Azure using documented instructions.
2. Users can authenticate using Microsoft Entra ID.
3. Authorized users can upload legal documents.
4. Documents are extracted, chunked, embedded, and indexed in Azure AI Search.
5. Users can ask questions about authorized documents.
6. Answers include valid citations to retrieved sources.
7. The assistant abstains when evidence is insufficient.
8. Matter-level access controls prevent unauthorized retrieval.
9. Users can upload supported audio files.
10. Azure Speech converts uploaded audio to text.
11. The platform generates structured transcript summaries.
12. Summaries include timestamp references where available.
13. Users can provide positive or negative feedback.
14. Feedback is stored with model, prompt, session, and retrieval metadata.
15. Semantic Kernel is used for at least three controlled tool calls.
16. SharePoint documents can be discovered or ingested through Microsoft Graph.
17. A Copilot Studio proof of concept is documented.
18. Terraform defines the required development infrastructure.
19. GitHub Actions validates and tests the application.
20. Application Insights records application and AI metrics.
21. The evaluation framework tests groundedness, hallucinations, citations, PII, and prompt injection.
22. Secrets are stored outside the repository.
23. Security and AI governance documentation is complete.
24. User and administrator guides are available.
25. The complete workflow can be demonstrated using a repeatable demo script.
26. A learning log explains the Azure services and skills learned.
27. Automated tests pass.
28. No critical security findings remain unresolved.
29. The project is suitable for demonstration during an AI Solutions Specialist interview.

---

# Suggested Jira-Style Subtasks

## AZURE-AI-01: Establish Azure Development Environment

Create the resource group strategy, naming conventions, tagging rules, budget alerts, and development environment.

## AZURE-AI-02: Provision Azure Resources with Terraform

Create Terraform modules for storage, Key Vault, database, AI Search, Speech, application hosting, monitoring, and Azure OpenAI or Foundry.

## AZURE-AI-03: Build Azure AI Chat Backend

Create the application backend and integrate it with an Azure-hosted model.

## AZURE-AI-04: Add Microsoft Entra ID Authentication

Implement authentication, roles, managed identity, and deny-by-default authorization.

## AZURE-AI-05: Implement Semantic Kernel Agent

Add controlled plugins for retrieval, document summarization, transcript summarization, and feedback.

## AZURE-AI-06: Build Document Ingestion Pipeline

Upload, extract, chunk, embed, index, and track legal documents.

## AZURE-AI-07: Implement Azure AI Search RAG

Create hybrid retrieval, semantic ranking, metadata filtering, citations, and abstention behavior.

## AZURE-AI-08: Add Matter-Level Access Controls

Ensure users retrieve documents only from matters they are authorized to access.

## AZURE-AI-09: Implement Audio Upload and Transcription

Upload recordings to Blob Storage and transcribe them with Azure Speech-to-Text.

## AZURE-AI-10: Implement Transcript Summarization

Generate summaries, action items, timelines, risks, deadlines, entities, and timestamp citations.

## AZURE-AI-11: Create Feedback and Conversation Database

Store sessions, messages, feedback, documents, transcripts, summaries, evaluations, and audit data.

## AZURE-AI-12: Integrate Microsoft Graph and SharePoint

Retrieve SharePoint documents while preserving source metadata and permissions.

## AZURE-AI-13: Build Copilot Studio Proof of Concept

Create a SharePoint-grounded copilot with at least one Power Automate action.

## AZURE-AI-14: Add AI Evaluation Framework

Create test datasets and evaluate groundedness, citations, hallucinations, PII, prompt injection, latency, and cost.

## AZURE-AI-15: Implement Security and Governance Controls

Add Key Vault, managed identity, PII protection, prompt-injection testing, audit logs, and an AI risk register.

## AZURE-AI-16: Add Monitoring and Cost Tracking

Instrument the system with Application Insights, Azure Monitor, dashboards, metrics, logs, and alerts.

## AZURE-AI-17: Create CI/CD Pipeline

Add GitHub Actions for formatting, linting, type checking, testing, security checks, Terraform validation, deployment, and smoke tests.

## AZURE-AI-18: Create Documentation and Training Material

Write architecture, setup, deployment, security, governance, user, administrator, troubleshooting, and demo documentation.

## AZURE-AI-19: Build Adoption and Quality Dashboard

Display usage, active users, feedback, failures, latency, cost, and evaluation results.

## AZURE-AI-20: Complete Final Product Demonstration

Run the end-to-end workflow and document lessons learned, architecture decisions, remaining gaps, and future improvements.

---

# Definition of Done

The document workflow must be demonstrable end to end:

```text
User signs in
→ selects an authorized legal matter
→ uploads or accesses a document
→ document is indexed
→ user asks a question
→ AI retrieves approved evidence
→ answer is generated with valid citations
→ user provides feedback
→ feedback and metrics are stored
```

The audio workflow must also be demonstrable:

```text
User uploads audio
→ audio is stored securely
→ Azure Speech creates a transcript
→ the transcript is summarized
→ action items and important details are extracted
→ timestamp citations are displayed
→ user reviews and rates the result
```

The completed project must include:

- Repeatable Azure deployment
- Automated tests
- Security controls
- Monitoring
- Evaluation results
- Technical documentation
- User documentation
- Learning documentation
- A polished interview demonstration
