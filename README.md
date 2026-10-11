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
# genai-student-study-assistant-