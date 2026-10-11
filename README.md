# 🎓 Generative AI Student Study Assistant Website

A 100% compute-isolated, offline, privacy-centric conversational academic platform engineered to allow students to interactively query dense textbooks and research papers using natural language without relying on cloud API networks.

## 👤 Academic Candidate Metadata
* **Student Name:** Akshitha
* **Institution:** Kommuri Pratap Reddy Institute of Technology (KPRIT)
* **Department Branch:** Computer Science and Engineering (Department of CSE-AIML)
* **Academic Cycle Matrix:** Pre-Final Year Phase – [2024–2028]
* **Project Mentor / Guide:** Raghavendra Rao Shambu Mahendrakar
* **Personal Repository Submission Path:** https://github.com

---

## 🛠️ System Features & Interactivity Matrix
1. **Compute-Isolated Architecture:** Runs completely offline via Ollama, removing cloud API fees, transaction logs, and external model limits.
2. **Dynamic Metric Analytics:** Updates the sidebar dashboard interface instantly upon upload to show total page metrics, word limits, and vector volume counts.
3. **Quick Action Study Shortcuts:** Built-in floating dashboard macro keys to auto-generate document summary templates, define technical terms, and compile exam review sheets with a single click.
4. **Session History Control:** A dedicated UI sidebar command trigger to instantly erase conversation histories and completely clear the text window chat canvas.

---

## 📊 Technical Stack Framework Mapping

| Architecture Layer Component | Technology Framework Employed | Local Operational System Responsibility | Network Reliance |
| :--- | :--- | :--- | :--- |
| **User Front Interface** | **Streamlit GUI Engine** | Renders dynamic dashboards, runs sidebar file loaders, and caches dialogue matrices using state dict handles. | **None (100% Offline)** |
| **Orchestration Parser** | **LangChain Library Core** | Segmentizes characters into 1,000 token boundaries with 200 overlaps, and binds context to grounding prompt templates. | **None (100% Offline)** |
| **Data Vector Database** | **ChromaDB / SQLite** | Converts strings into 384-dimensional floating point coordinates and handles sub-second cosine distance matching. | **None (100% Local)** |
| **Local Compute Engine** | **Ollama LPU Controller** | Runs 4-bit quantized open-weight networks (**Llama 3 8B**) completely offline on native host laptop hardware. | **None (100% Edge-Private)** |

---

## 💻 Local Windows Workspace Terminal Launch Guide

Follow these sequential steps inside your Windows Command Prompt (`cmd`) to boot up the interactive application on your system:

### 1. Navigate into your local student assistant project directory folder
```cmd
cd path/to/genai-student-study-assistant-
```

### 2. Ingest package requirements via python pip environment managers
```cmd
pip install -r requirements.txt
```

### 3. Download foundational model weights directly onto system disk storage (Ensure Ollama is running)
```cmd
ollama pull llama3:8b
```

### 4. Spin up the Streamlit host web server application
```cmd
streamlit run app.py
```
*Once executed, your web browser will automatically open your interactive tutor workspace portal at `http://localhost:8501`.*

---

## 📁 COMPLETE PROJECT APPLICATION SOURCE CODE

### 📄 1. Configuration Package Manifest: `requirements.txt`
```text
streamlit==1.32.0
langchain==0.1.12
langchain-community==0.0.28
chromadb==0.4.24
sentence-transformers==2.5.1
pypdf==4.1.0
python-dotenv==1.0.1
```

### 📄 2. Core Retrieval Backend Logic Layer: `rag_pipeline.py`
```python
# rag_pipeline.py - Core data pipeline and local vector logic layer
import os
from langchain_community.document_loaders import PyPDFLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_community.embeddings import HuggingFaceEmbeddings
from langchain_community.vectorstores import Chroma
from langchain_community.llms import Ollama

class StudyAssistantPipeline:
    def __init__(self):
        # Local 384-dimensional open-source transformer model to generate numeric coordinates
        self.embeddings = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
        self.persist_directory = "./chroma_db"
        # Connect to local Ollama service running Llama 3
        self.llm = Ollama(model="llama3:8b", temperature=0.3)
        
    def process_pdf(self, file_path: str):
        """Extracts text tracks page-by-page, sets chunk boundaries, and handles ChromaDB storage."""
        loader = PyPDFLoader(file_path)
        documents = loader.load()
        
        # Calculate dynamic analytics metrics for the frontend interface panel
        total_pages = len(documents)
        total_words = sum(len(doc.page_content.split()) for doc in documents)
        
        # Segment character structures into 1,000 token spans with a 200 token overlap
        text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
        chunks = text_splitter.split_documents(documents)
        
        # Build and update the local database vector store configuration directories on disk
        vector_store = Chroma.from_documents(
            documents=chunks, embedding=self.embeddings, persist_directory=self.persist_directory
        )
        vector_store.persist()
        
        return {"chunks": len(chunks), "pages": total_pages, "words": total_words}

    def query_assistant(self, user_question: str) -> str:
        """Pulls top matching context rows and executes grounded offline text inference loops."""
        if not os.path.exists(self.persist_directory):
            return "System Notice: Please upload an academic textbook PDF file first."
            
        vector_store = Chroma(persist_directory=self.persist_directory, embedding_function=self.embeddings)
        retriever = vector_store.as_retriever(search_kwargs={"k": 3})
        relevant_docs = retriever.get_relevant_documents(user_question)
        context_text = "\n\n".join([doc.page_content for doc in relevant_docs])
        
        # Strict context prompt boundaries template to eliminate model hallucinations
        prompt = (
            f"You are a professional Generative AI Student Study Assistant.\n"
            f"Your goal is to explain complex academic concepts simply using clear bullet points.\n"
            f"Base your answer strictly on the provided context text block. If the context does not contain "
            f"the answer, say 'I cannot find that specific information in your uploaded document.'\n\n"
            f"STUDY CONTEXT:\n{context_text}\n\n"
            f"STUDY QUESTION: {user_question}\n\n"
            f"ANSWER:"
        )
        return self.llm.invoke(prompt)
```

### 📄 3. Front Dashboard Web Interface: `app.py`
```python
# app.py - Stateful user interface controller dashboard layout
import streamlit as st
import os
from rag_pipeline import StudyAssistantPipeline

st.set_page_config(page_title="Interactive GenAI Tutor", page_icon="🎓", layout="wide")

st.markdown("""
<style>
@import url('https://googleapis.com');
.block-container {padding-top: 1.5rem; max-width: 1250px;}
.stApp h1, .stApp h2, .stApp h3 {font-family: 'Poppins', sans-serif; color: #1E3A8A;}
section[data-testid="stSidebar"] { background: linear-gradient(180deg, #F0FDFA, #CCFBF1); }
button {border-radius: 12px !important; font-weight: 600 !important;}
.stMetric {background-color: #FFFFFF; padding: 10px; border-radius: 10px; border: 1px solid #E2E8F0;}
footer {visibility: hidden;}
</style>
""", unsafe_allow_html=True)

st.title("🎓 Interactive GenAI Student Study Assistant Website")
st.subheader("Compute-Isolated Academic Tutor. 100% offline, privacy-centric workspace.")

pipeline = StudyAssistantPipeline()

if "chat_history" not in st.session_state: st.session_state.chat_history = []
if "doc_metrics" not in st.session_state: st.session_state.doc_metrics = None
if "triggered_preset" not in st.session_state: st.session_state.triggered_preset = None

with st.sidebar:
    st.header("📂 Document Ingestion Node")
    uploaded_file = st.file_uploader("Upload reference file (PDF format only):", type=["pdf"])
    
    if uploaded_file:
        temp_file_path = os.path.join("./", uploaded_file.name)
        with open(temp_file_path, "wb") as f: f.write(uploaded_file.getbuffer())
        with st.spinner("Parsing text tracks and generating vector spaces..."):
            st.session_state.doc_metrics = pipeline.process_pdf(temp_file_path)
            st.success("✓ Document processed successfully!")
            os.remove(temp_file_path)
            
    if st.session_state.doc_metrics:
        st.markdown("---")
        st.subheader("📊 Document Live Analytics")
        st.metric("Total Pages Indexed", st.session_state.doc_metrics["pages"])
        st.metric("Total Words Parsed", f"{st.session_state.doc_metrics['words']:,}")
        st.metric("Search Vectors Generated", st.session_state.doc_metrics["chunks"])
        
    st.divider()
    st.info("👤 **Student:** Akshitha\n\n⚙️ **Engine:** Ollama Llama 3 Offline")
    
    if st.button("🗑️ Clear Chat History", use_container_width=True):
        st.session_state.chat_history = []; st.session_state.triggered_preset = None; st.rerun()

if st.session_state.doc_metrics:
    st.write("### ⚡ Quick Study Actions")
    col1, col2, col3 = st.columns(3)
    if col1.button("📝 Summarize Document Core Thesis", use_container_width=True):
        st.session_state.triggered_preset = "Provide a high-level summary of the core thesis of this uploaded document."
