# Pathfinder AI 🤖

An AI-Powered Career Guidance System built with **LangChain**, **Mistral-7B**, and **RAG** for a B.Tech Minor Project.

This project uses a Retrieval-Augmented Generation (RAG) pipeline to function as an intelligent career counsellor. It analyzes a student's profile, interests, and constraints (like "dislikes math") to provide a personalized, logical, and actionable career plan.

---

## 🚀 Key Features

* **Personalized Career Analysis:** Generates 2-3 genuinely suitable career roles based on a user's complex profile.
* **Skill-Gap Analysis:** Identifies and lists 3 key missing skills for each recommended role.
* **Custom Learning Roadmap:** Provides a structured 6-month learning plan with suggested courses and projects.
* **RAG with Web Search:** Combines a local knowledge base (FAISS vector store) with real-time web results (Serper API) for context-aware, up-to-date answers.
* **Intelligent Reasoning:** Uses advanced prompt engineering to critically evaluate user constraints, preventing bad recommendations (e.g., suggesting a math-heavy "AI Engineer" role to a user who dislikes math).

---

## 📸 Demo

Here is the final Streamlit application interface, deployed via Ngrok.



Here is an example of the structured output for a user.



---

## 🛠️ Tech Stack & Architecture

This project is a complete RAG pipeline orchestrated by **LangChain (LCEL)**.

* **Framework:** LangChain
* **LLM (Generator):** `mistralai/Mistral-7B-Instruct-v0.3`
* **LLM Optimization:** 4-bit Quantization (`BitsAndBytes`)
* **Retrieval (Vector DB):** `FAISS` (Facebook AI Similarity Search)
* **Embeddings:** `sentence-transformers/all-MiniLM-L6-v2`
* **Web Augmentation:** Google Serper API
* **Front-End:** Streamlit
* **Deployment:** Google Colab (as host) & Ngrok (for tunneling)

### How It Works

1.  **User Input:** The user provides their profile in the **Streamlit** UI.
2.  **RAG Chain Invocation:** The **LangChain** RAG chain is triggered.
3.  **Parallel Retrieval:** Using `RunnableParallel`, the system fetches context from two sources at once:
    * **Local Data:** The **FAISS** vector store is queried for semantically similar job roles from the internal dataset.
    * **Web Data:** The **Serper API** is queried for real-time info (like salaries or trends).
4.  **Prompt Augmentation:** The retrieved local and web context is injected into a **smart system prompt**.
5.  **Generation:** The complete prompt is sent to the 4-bit quantized **Mistral-7B** model, which generates the structured, logical career plan.



---

## ⚙️ Setup & Installation

This project is designed to run in a **Google Colab** environment to leverage its free T4 GPU.

### 1. Clone the Repository
```bash
git clone [https://github.com/your-username/pathfinder-ai.git](https://github.com/your-username/pathfinder-ai.git)
cd pathfinder-a
```
### 2. Required Packages
```bash
pip install -q \
    langchain \
    langchain-community \
    langchain-huggingface \
    transformers \
    sentence-transformers \
    faiss-cpu \
    accelerate \
    bitsandbytes \
    requests==2.32.4 \
    huggingface-hub \
    google-search-results \
    streamlit \
    pyngrok
```
### 3. Set Up API Keys
* HF_TOKEN: Your Hugging Face read token (to access the Mistral-7B model).

* SERPER_API_KEY: Your API key from Serper.dev (for the web search).

* NGROK_AUTH_TOKEN: Your authentication token from the Ngrok dashboard.
### 4. Run the Application
```bash
# Kill any old processes
!killall streamlit >/dev/null 2>&1

# Set environment variables from secrets
import os
from google.colab import userdata
os.environ['HF_TOKEN'] = userdata.get('HF_TOKEN')
os.environ['SERPER_API_KEY'] = userdata.get('SERPER_API_KEY')
NGROK_TOKEN = userdata.get('NGROK_AUTH_TOKEN')

# Launch Streamlit in the background
!env HF_TOKEN=$HF_TOKEN SERPER_API_KEY=$SERPER_API_KEY nohup streamlit run app.py --server.port 8501 &

# Start and authenticate Ngrok
from pyngrok import ngrok
ngrok.set_auth_token(NGROK_TOKEN)
public_url = ngrok.connect(addr='8501', proto='http')
print(f"🚀 Pathfinder AI is live at: {public_url}")
```
### 📜 License
This project is open-source under the MIT License. see the [MIT License](LICENSE) file for details.
