 https://www.youtube.com/watch?v=lG7Uxts9SXs

LangChain is a framework designed to simplify the creation of applications using Large Language Models, that makes it easy to connect AI models with a bunch of different data sources so that we can create customized NLP applications.

Components: 
- LLM Wrappers: allow us to connect to a Large Language Models like GPT4, Hugging Face
- Prompt Templates: allow us to avoid having to hardcode text which is the input to LLMs
- Indexes for information retrieval: allow us to extract relevant information for the LLMs

Chains: allow us to combine multiple Components together to solve a specific task and build an entire application

Agents: allow LLMs to interact with its environment and any of the external API's

```
python -m venv .venv
.venv/Scripts/Activate.ps1
pip install langchain openai streamlit python-dotenv
```

streamlit allows us to build interface for Python applications

inside dotenv file, our Open AI API key will reside as environment variable

python main.py

---

https://www.youtube.com/watch?v=bcCFTk_uwUw

https://python.langchain.com/docs/tutorials/llm_chain/

