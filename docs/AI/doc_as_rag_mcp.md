# 用 LangChain + Ollama(nomic-embed-text) + MCP 實現本地端的內部文件查詢

目標：用原始 markdown 文件，透過 LangChain，打造一個 MCP server，讓 Cline 或 Copilot 能掛載並查詢（RAG）文件內容。

## 預計步驟

**步驟 0：環境建置**
- 安裝 Ollama 以及抓特定的模型

**步驟 1：Markdown 資料預處理**
- 清理並整理 markdown 文件（移除不必要文字、格式統一）
- 按照標題、章節進行 chunk（分割）並保存原始 meta 資訊（如檔名、標題）

**步驟 2：建立文檔索引與向量資料庫**
- 將每個 chunk 轉成 embedding 向量
- 選用一個向量資料庫（如 FAISS、Chroma、Weaviate），將 chunk 存入，並保留 meta

**步驟 3：LangChain RAG Pipeline 開發**
- 用 LangChain 建立 Retriever：根據 query 查找與之最相關 chunk
- 構建 Prompt 組裝邏輯：將檢索片段與用戶 query 組合後餵入 LLM
- 設計分段回覆格式（如 markdown to HTML，方便外部客戶端展現）

**步驟 4：MCP Server 架構設計與 API 開發**
- 開發 MCP server（建議用 FastAPI/Python，易搭配 LangChain）
- 規劃 API 結構，例如：
    - `/query`：接收 query 字串，返回 RAG 回答（含 chunk 細節）
    - `/chunks`：資料分段及索引維護
    - `/embedding`：管理與更新向量資料
- 支援 Cline 或 Copilot 掛載的協定（如 RESTful、gRPC 或 OpenAI Plugin 標準）

**步驟 5：整合與掛載驗證**
- 提供如 OpenAI Copilot 或其他 agent 的掛載教學
- 實測端到端流程（query → MCP Server → RAG → response）

**步驟 6：持續優化與維護**
- 增加資料內容自動同步（如有後續 markdown 更新）
- 日誌與效能監控
- security/cross-origin 設定，保護 API

## 詳細步驟拆解

### 步驟 0

準備以下環境：
- 透過 winget 安裝 ollama
- 透過 `ollama pull` 下載模型 nomic-embed-text
- 使用 Chroma 作為向量資料庫

利用 LangChain 封裝的 ollama 套件，直接指定 Ollama 的 HOST URL 以及選用的嵌入模型，以及透過建立 Chroma 向量資料庫的實體。
```python
from langchain_ollama import OllamaEmbeddings
from langchain_chroma import Chroma

# Constants
CHROMA_DB_PATH = os.getenv("CHROMA_DB_PATH", "./chroma_db")
EMBEDDING_MODEL = os.getenv("EMBEDDING_MODEL", "nomic-embed-text")
OLLAMA_HOST = os.getenv("OLLAMA_HOST", "http://localhost:11434")

def get_embeddings():
    """Initializes and returns the Ollama embeddings."""
    return OllamaEmbeddings(model=EMBEDDING_MODEL, base_url=OLLAMA_HOST)

def get_vector_store(embeddings):
    """Initializes and returns the Chroma vector store."""
    return Chroma(persist_directory=CHROMA_DB_PATH, embedding_function=embeddings)
```

### 步驟 1

在分割文件之前，我們需要先按照文件類型選擇適合的載入器，以這次匯入的資料是 markdown 類型的文件，可以直接使用 **TextLoader**。

```python
from langchain_community.document_loaders import (
    DirectoryLoader,
    PyPDFLoader,
    TextLoader,
)

def load_documents_from_path(path):
    print(f"Loading document from file: {path}")
    try:
        _, extension = os.path.splitext(path)
        extension = extension.lower()
        if extension == ".pdf":
            loader = PyPDFLoader(path)
        elif extension in [".txt", ".md", ".py", ".json", ".html", ".csv"]:
            loader = TextLoader(path, encoding="utf-8")
        else:
            # Fallback to UnstructuredLoader for other file types
            loader = UnstructuredLoader(path)
        return loader.load()
    except Exception as e:
        print(f"Error loading file {path}: {e}")
        return []
```

### 步驟 2

在成功載入文件之後，要將文本分割成較小且語意完整的塊，可以讓向量代表更精確的語義片段，有助於在檢索時找到更相關的內容。設定 overlap 是為了在有限的文本區塊大小下，確保語義和上下文連續不被割裂，提高向量檢索與生成效果的關鍵設置。

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

TEXT_CHUNK_SIZE = int(os.getenv("TEXT_CHUNK_SIZE", "1000"))
TEXT_CHUNK_OVERLAP = int(os.getenv("TEXT_CHUNK_OVERLAP", "200"))

def process_and_store_documents(documents, vector_store):
    """
    Splits documents into chunks and stores them in the vector store.
    """
    if not documents:
        print("No documents to process.")
        return

    text_splitter = RecursiveCharacterTextSplitter(
        chunk_size=TEXT_CHUNK_SIZE, chunk_overlap=TEXT_CHUNK_OVERLAP
    )
    splits = text_splitter.split_documents(documents)

    print(f"Splitting documents into {len(splits)} chunks.")

    vector_store.add_documents(documents=splits)
    print("Documents have been successfully processed and stored in ChromaDB.")
```

### 步驟 3

### 步驟 4

### 步驟 5

### 步驟