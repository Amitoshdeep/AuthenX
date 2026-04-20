# Hack Helix

Hack Helix is an advanced dual-model AI orchestration system developed as a hackathon project. It focuses on solving large language model hallucinations and formatting inconsistencies by implementing a robust Generator-Validator architecture.

## Project Description

The platform allows users to submit prompts alongside specific tool contexts (e.g., charts, documents, flowcharts, forms). Rather than returning immediate, unverified generative output, the system runs an automated orchestration loop. The Generator creates the requested structured output, and the Validator rigorously evaluates it for factual accuracy, formatting compliance, and citation validity using real-time search. The loop continues until the Validator passes the output or a maximum iteration limit is reached, ensuring reliable and high-quality results.

## System Architecture

### High-Level Components

mermaid
flowchart LR
    A[React 19 Frontend] <-->|API Requests| B[FastAPI Backend]
    B <-->|Orchestration| C[LangChain / Bedrock]

    subgraph Frontend Renderers
    R1[Chart.js]
    R2[React Force Graph]
    R3[Mermaid.js]
    end

    A --> Frontend Renderers

    subgraph AI Models
    M1[Generator Model]
    M2[Validator Model]
    end

    C --> M1
    C <--> M2
    M2 <-->|Fact-Checking| W[(SerpAPI Web Search)]


### The Generator-Validator Loop

mermaid
flowchart TD
    Start([User Input: Prompt + Tool]) --> Orch[Orchestrator]
    Orch --> Gen[Generator Model]
    Gen -. Output .-> Orch
    Orch --> Val[Validator Model]
    Val <--> Search[(Web Search)]
    Val -. Evaluate .-> Decision{Pass Validation?}
    Decision -- No / Feedback --> Orch
    Decision -- Yes / Pass --> Final[Push Validated Output to Client]
    Decision -- Max Rounds Reached --> Final


1. *Generator Model*: Produces the initial content strictly bounded by the requested JSON schema or formatting skill.
2. *Validator Model*: Acts as a strict evaluator. It utilizes a LangChain-powered agent with access to live web search via SerpAPI to fact-check claims, verify URLs, and detect hallucinations.
3. *Orchestrator*: A Python-based controller state machine that manages the conversational history, feedback loops, and round attempts between the Generator and Validator.
4. *Dynamic Frontend Renderers*: Once validated, the JSON payload is dispatched to the client where React components parse and natively render interactive charts, force-directed graphs, Mermaid diagrams, or downloadable files.

## Tech Stack

### Frontend
* *Core*: React 19, Vite
* *Styling & Animation*: TailwindCSS, Shadcn UI, Radix UI, Framer Motion, GSAP, Three.js

### Backend
* *API Framework*: Python, FastAPI, Uvicorn
* *AI & Orchestration*: Amazon Bedrock, LangChain, Boto3
* *Validation Tools*: SerpAPI (Real-time Web Search)
* *Document Generation*: FPDF2, Python-docx
* *Database*: Amazon DynamoDB

## Team

* *Amitoshdeep*: Frontend
* *Gurveer Pratap*: Backend
* *Jiya Gupta*: UI/UX
* *Jasmeet Kaur*: Presentation, Script, and Research
