https://harshad-inarkar.github.io/project_group_recsys/

Password: iitb

---

Final Project Deliverables & Faculty Guidelines

You are required to submit three deliverables for your final project:

1. A Video Presentation (maximum 10 minutes in length)
2. Presentation Slides
3. A Detailed Project Report

(Note on Code: You are not required to submit your code. Whether you keep your repository private or make it public is entirely your choice.)

1. Video & Presentation Slides Guidelines
- Recording Format: Use your presentation slides to record your video. Walk through and explain your slides alongside live/recorded demonstrations of your work.
- Proof of Claims: In the video, you must demonstrate everything you claim. Unsupported claims will not be appreciated. Focus on clearly showcasing what you have actually achieved in the project.
- Recommended Slide Structure: While you have creative freedom over how you design your slides and video, your presentation should typically follow this flow:
  1. Problem Statement
  2. Your Approach to Solve the Problem (overall idea)
  3. Steps of the Solution (Most important section — provide clear details on data collection, model selection, training, deployment, etc.)
  4. Results Achieved

2. Project Report Guidelines
- Depth & Structure: The report must be elaborate and organized into multiple chapters, bringing out all technical and implementation details as thoroughly as possible.
- Replicability Standard: Your report should be detailed enough that anyone reading it can replicate your work from scratch.
- Clarity & Readability: As you write, continuously ask yourself: "Will the person reading this understand what I am trying to say?" It is your responsibility to present the methodology, decisions, and findings clearly so the reader can follow your work effortlessly.

---

**Target implementation**
```
Customer Query
      |
      v
Retrieve Relevant Documents
      |
      v
Pass Context to LLM
      |
      v
Generate Grounded Response
      |
      v
Validate Response
      |
      v
Return to Customer
```

| Technology | What it does |
| :--- | :--- |
| Python | Main programming language |
| FastAPI | Exposes our backend REST APIs |
| Groq | Provides cloud-based LLM inference through API |
| OpenAI GPT-OSS 20B | The language model used to generate customer support responses |
| Groq Python SDK | Connects our Python application to Groq API |
| Pydantic | Validates API requests and responses |
| python-dotenv | Loads environment variables and securely manages API configuration |
| Uvicorn | Runs our FastAPI application |
| VS Code | Development environment |
| LangGraph | Orchestrates multiple AI agents |
| LangChain | Provides components for RAG and LLM integration |
| FAISS | Stores and retrieves document embeddings |
| SQLite | Stores customer conversations and support tickets |
| Streamlit | Provides the customer-facing interface |
| Pytest | Tests application functionality |

Important distinction: Groq is the inference provider, whereas GPT-OSS 20B is the actual language model. They serve different roles in our architecture.

| Python implementation | Java/Spring Boot equivalent |
| :--- | :--- |
| FastAPI() | @SpringBootApplication |
| @app.post("/chat") | @PostMapping("/chat") |
| BaseModel | Request DTO |
| Groq() | External API client |
| client.chat.completions.create() | REST client call |
| CustomerResponse | Response DTO |
| HTTPException | ResponseStatusException |

<img width="550" height="306" alt="image" src="https://github.com/user-attachments/assets/a6d4cfd1-9fc3-4680-bc33-95dd88a6cab7" />

| Approach | How it works |
| :--- | :--- |
| Basic LLM | Generates responses using its learned knowledge |
| Prompt engineering | Uses instructions to constrain responses |
| RAG | Retrieves external knowledge and supplies it to the LLM |
