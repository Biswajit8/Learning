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

https://www.youtube.com/watch?v=7TdOwFcLV5s

We’ll build a complete RAG pipeline with Spring Boot
- Vector Embeddings
- Using Postgres as a vector database
- Using Spring AI to generate responses with Gemini and RAG

### What are Vector Embeddings?

How can AI models understand the meaning and relationship behind words?
The magic behind this is a concept called Vector Embeddings. It's just a group of numbers that form a numerical representation of data, in this case text. You can think of it like a special translator. You give it a word or a sentence and it translates that text into a list of numbers which make up our vector. The cool thing about this is that these values are actually meaningful. So words or phrases with similar meanings have vectors that are numerically closer to each other. In the real world, these vectors can have hundreds or even thousands of dimensions. But to make it easier to understand, let's imagine a model that creates vectors with only two dimensions, so we can plot any piece of text on a simple X&Y graph. So if we had to plot the vectors for dog, cat, bark and meow, the points for dog and bark would be close and cat and meow would be close as well. This shows that the model understands that dogs are related to barking and cats are related to meowing. The distance between any 2 vectors shows how closely related the two pieces of text are. 

So how do you create these embeddings? Using AI models that have been trained for this purpose. For e.g., LM Studio.

If we have a search query and want to find relevant text that's related to it, we need to find the distance between the search query vector and the vectors of the text that we have in our database.

Behind the scenes, Spring AI retrieved the documents from the pgvector database, sent them along with our question through the AI model via OpenRouter, which then generated this response.

### Things to consider if we want to create this kind of system in a production environment:
1. In our example we use pretty small snippets for each of our documents. If you have a large document, the similarity search may not work as well and you may have to take that document down into smaller pieces. This is known as document chunking and each of these pieces or chunks can then be stored as a separate entry in your vector store. 
2. Another thing to think about is the quality of your embeddings. The nomic embed text model that we use here is great, but the performance of your RAG system is highly dependent on how well your embedding model understands the nuances of your specific data. So you may want to experiment with other embedding models.
