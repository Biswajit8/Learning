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

If someone asks how you chose your chunk size, you can say you inspected the chunks, found title-only and mid-rule splits, and fixed them with structure-aware splitting plus metadata.

turn chat() into a LangGraph workflow:
```
                     ┌─────────────► knowledge ──► resolution ──► END
START ──► planner ───┼─────────────► order_status ───────────────► END
                     └──► escalation ◄── (no policy / not covered / delayed order)
```

<img width="520" height="652" alt="image" src="https://github.com/user-attachments/assets/35094eae-4457-4969-ac8e-e63d4c892990" />

solid arrows are fixed transitions, dotted arrows are conditional routes decided at runtime, and escalation is reachable from the planner, knowledge, resolution and order_status nodes.

wires it into the graph:
- /chat accepts an optional session_id, loads that session's history, and saves both sides of the exchange.
- The planner sees the history, so a bare "ORD1001" after "Where is my order?" is classified as order_status.
- The order-status node falls back to earlier messages to find an order ID.
- The escalation node creates a ticket and tells the customer the ticket number.
- A new GET /tickets endpoint lets a human agent see open tickets, and Streamlit can reuse it.

Two notes
1. Every escalation in eval_repeat.py now creates a ticket, so support.db will fill with test tickets. Delete support.db before demos. It's recreated automatically on startup.
2. The order-status node reuses the most recent order ID in the session. If a customer later asks about a different order without giving its ID, they'd get the earlier one. That's fine for a demo, but worth stating in your limitations section.

It's worth mentioning in your presentation that sessions are isolated by ID. You can also confirm it in the sidebar. The session ID shown there should change every time you click the button.

Test the six scenarios from your project brief:
- My product arrived damaged. What should I do?
- Can I return a product after 15 days?
- Where is my order? (then reply ORD1001)
- I want to cancel my order.
- I received a refund, but the amount is incorrect.
- I am unhappy with your service. I want to speak to a human.

Testing:
- Regression protection: The title-only chunk bug is now a permanent test, and so are the isolation and routing rules.
- A clear split: Offline tests verify your own logic deterministically. The LLM tests measure model behavior, which can vary between runs. That distinction is worth stating in your presentation.
- Safe test data: Every test runs against a throwaway database, so support.db and your demo tickets stay untouched.

Evaluation script:
- Retrieval quality: whether the correct policy file is the top result (top-1) or among the results (any-of-k).
- Planner accuracy: whether the predicted intent matches the expected one.
- Escalation accuracy: whether the system answers or escalates correctly.
- Fact accuracy, RAG vs plain LLM: whether answers contain the facts the policy states ("10 days", "48 hours", and so on), compared with the Day 1 setup that has no retrieval.

Limits:
- You wrote both the policies and the test questions, so the dataset is friendly. A real evaluation would use questions from real customers.
- Keyword matching is a proxy for correctness. It confirms the key fact is present, not that the whole answer is faithful. A manual review of a few answers is worth adding.
- 22 questions is a small sample, so treat the percentages as indicative, not precise.

- the first evaluation run reported 93% fact accuracy. Inspecting the failing answer showed that the model's output used a Unicode narrow no-break space, which defeated exact string matching. You normalized text in the checker and re-ran. That's a true account, and it shows you examined a failure before deciding what it meant. It also supports a point about LLM evaluation in general: output text can differ from what you see on screen, so checkers need normalization.
- Fixing the checker raised the plain-LLM baseline from 21% to 39%, because the same Unicode-space problem was understating it too. RAG now scores 28/28 (100%) vs 11/28 (39%). The baseline still gets some right by coincidence on generic terms like "email" or "replacement". The real signal is that without retrieval it can't state your specific rules (a 10-day window, 48-hour reporting, the Rs 100 fee).

Check these before you commit:
- Files listed in the structure tree: Remove any that don't exist in your folder, and add any I missed.
- Setup details: The README says Python 3.11+ and a ~70 MB embedding download (your log showed 67 MB). Both match what you saw, but confirm them.
- Evaluation table: It uses your final numbers. If you re-run run_eval.py with extra hard cases, update the table with whatever you get, even if it's lower. A "known weaknesses" line built from real failures makes the project more credible.
- Limitations: I wrote these from what we found. Add anything you've noticed yourself.

Presentation preparation:
- Why RAG? Your Day 1 experiment showed the model inventing a refund policy.
- How did you choose the chunking and the threshold? You inspected chunks and measured scores.
- How do you stop hallucination? The prompt rules help, but the wrong-refund regression showed they aren't enough, so escalation is routed deterministically.
- How did you evaluate it, and what are the weak spots? You found a bug in your own checker, and the test set is small and synthetic.

Presentation prep:
- Problem (30 s). On Day 1 the plain LLM invented a refund policy, with a timeline and a phone number. That's unacceptable for support.
- Approach (60 s). RAG for grounding, plus a LangGraph workflow so decisions are explicit, with escalation as a first-class outcome.
- Three things you learned (90 s). Chunking looked fine until you read the output (title-only chunks). The prompt rule alone wasn't enough to make escalation reliable, so it became a routing decision. Your own evaluation checker had a bug that hid correct answers.
- Results (60 s). The table, with the 100% vs 39% comparison, and an honest line about the small, synthetic test set.
- Live demo (60 s). Damaged item, then the order-ID follow-up, then an escalation with a ticket.

