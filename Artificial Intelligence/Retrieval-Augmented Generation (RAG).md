https://www.youtube.com/watch?v=hYZKrPOyEYk

LLM (Large Language Model) simply predicts the next token along with probability. For eg:
```
User: The cat sat on the
AI Assistant: The cat sat on the mat
```

Retrieval-Augmented Generation (RAG) is helpful since LLM has drawbacks - LLM has Context Window. The Context Window is the maximum number of tokens that you can provide to it at one time for processing. For eg, GPT-4o can process 128k tokens at a time.
```
The cat sat on the mat
6 tokens
```

Retrieval-Augmented Generation (RAG) is an AI framework that enhances large language models (LLMs) by connecting them to external data sources, like a knowledge base or a company's internal documents, before generating a response. This process improves the accuracy, relevance, and timeliness of AI outputs by providing the LLM with context from authoritative, up-to-date information, thus reducing errors, "hallucinations," and the need for costly model retraining.
```
User: Who won the India vs England match in 2024
```

LLM initially might not know the answer. But after we feed relevant data to LLM, it will provide the relevant answer:
```
In the 2024 match between India and England, India won.
```

RAG makes LLMs more explainable.

If you're building a tool to help customers with your product, or an internal tool for company specific data, you can't use public AI models directly because their knowledge is limited to the publicly available information that they are trained on. RAG provides a way to give the AI a repository of our specific information that it can use as a reference to answer questions or generate information. 

### Real world applications based on RAG: 
- Cursor AI which auto-completes code in IDE
- Google AI Mode
- Online email generators
- Omnisend (a marketing automation platform for ecommerce businesses, focusing on email and SMS marketing)
- Harvey AI (for law)

### How RAG works?

Steps:
1. Query Input - The user submits a question or prompt to the RAG system, which processes and understands the intent to determine what type of information needs to be retrieved.
2. Retrieval - The system searches external knowledge sources (databases, documents, vector stores) to find and fetch the most relevant information that matches the user's query using semantic similarity or keyword matching.
3. Augmentation - The original user query is enhanced by combining it with the retrieved relevant information, creating an enriched prompt that provides additional context to the language model.
4. Generation - The large language model uses both the original query and the retrieved contextual information to generate a comprehensive, accurate, and contextually grounded response that addresses the user's question.

---

We’ll build a complete RAG pipeline with Spring Boot
- Vector Embeddings
- Using Postgres as a vector database
- Using Spring AI to generate responses with Gemini and RAG

What are Vector Embeddings?
