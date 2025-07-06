
# 🧠 RAG + FAISS + Groq: Smart Document Summarizer

A powerful Colab-based pipeline that uploads documents (PDF, DOCX, PPTX, TXT), embeds them using Sentence Transformers, retrieves top-matching chunks with FAISS, and summarizes them using **Groq's ultra-fast LLMs (like LLaMA3)**.

---

## 🚀 Features

- 📄 **Multi-format Upload**: Supports `.pdf`, `.docx`, `.pptx`, `.txt`
- 🧠 **Semantic Embeddings**: Uses `all-MiniLM-L6-v2` from `sentence-transformers`
- 🔍 **Efficient Retrieval**: FAISS-powered vector similarity search
- 🤖 **Fast Summarization**: Generates a summary using Groq’s blazing-fast LLaMA3 model
- ☁️ **Runs on Google Colab**: Zero local setup required

---

## 🔧 Setup Instructions

### 1. 🛠 Install dependencies
```python
!pip install numpy==1.24.4
!pip install faiss-cpu
!pip install sentence-transformers
!pip install pymupdf python-pptx python-docx pandas groq
```

### 2. 📤 Upload your documents
```python
from google.colab import files
uploaded = files.upload()
```

### 3. 🧹 Reinstall PyMuPDF correctly (to avoid `fitz` conflicts)
```python
!pip uninstall -y fitz
!pip install pymupdf
```

---

## 📚 File Handling

Supports extracting text from:
- ✅ PDF (`PyMuPDF`)
- ✅ PowerPoint (`python-pptx`)
- ✅ Word Documents (`python-docx`)
- ✅ Text Files (`.txt`)

---

## 📈 Embedding & Retrieval Logic

- Documents are encoded using `SentenceTransformer("all-MiniLM-L6-v2")`
- Indexed via FAISS (`IndexFlatL2`)
- A search query retrieves top-3 semantically similar documents

---

## 🧠 LLM Summarization with Groq

After retrieval:
```python
from groq import Groq

client = Groq(api_key="YOUR_GROQ_API_KEY")
prompt = "Summarize the following:\n\n" + "\n\n".join(similar_docs)

response = client.chat.completions.create(
    model="llama3-8b-8192",
    messages=[{"role": "user", "content": prompt}]
)

print("🧠 Summary:")
print(response.choices[0].message.content)
```

---

## 🔐 API Key

To use Groq’s LLMs, sign up at [Groq Cloud](https://console.groq.com/) and get your API key.

```python
groq_api_key = "YOUR_GROQ_API_KEY"
```

---

## 📊 Example Use Case

Imagine you upload:
- `Research_Paper.pdf`
- `Meeting_Notes.docx`
- `Slides.pptx`

Then ask:  
> "Summarize the key insights from the uploaded documents"

The model will:
1. Find top 3 relevant files using FAISS
2. Feed them to LLaMA3 via Groq
3. Return a clear, crisp summary 🔥

---

## 🧠 Built With

- `sentence-transformers`
- `faiss-cpu`
- `groq`
- `PyMuPDF`, `python-docx`, `python-pptx`
- Google Colab
