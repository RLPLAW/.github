# Projects Overview: RAG-Law & RAG UI

This document provides an overview of two projects:

1. **RAG-Law** – A Retrieval-Augmented Generation (RAG) system for processing and searching legal documents.  
2. **RAG UI** – A cross-platform Avalonia application providing a responsive desktop and mobile interface.

---

## 1. RAG-Law

### Overview

RAG-Law processes PDF legal documents, stores their text embeddings in **ChromaDB**, and uses **Ollama** to answer queries based on the stored data. Designed for **Windows**, it uses Python 3.13.4, Ollama 0.9.0, ChromaDB, and Redis.

> **Note:** This project is not tested on macOS.

---

### Main Requirements

- **Python 3.13.4** (added to PATH)  
- **Ollama 0.9.0** ([Download](https://ollama.com/download))  
- **ChromaDB** (via Docker)
  
#### Installing ChromaDB & Redis:

```bash
# ChromaDB
docker run -d --name chroma-vectors -p 8000:8000 chromadb/chroma:latest

# Redis
docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
````

#### Python Dependencies:

```bash
pip install sentence-transformers python-docx PyPDF2 redis langchain langchain-ollama python-dotenv langchain-community chromadb
```

Or via `requirements.txt`:

```bash
pip install -r requirements.txt
```

#### Pull Ollama Models:

```bash
ollama pull llama3.1
ollama pull nomic-embed-text
```

---

### Project Structure

```
RAG-Law/
├── code/
│   ├── core/
│   │   └── vector/
│   │       ├── chroma_client.py
│   │       ├── ollama_interaction.py
│   │       └── main.py
│   └── ...
├── AI_DOCS_TEST/
│   └── Reports/   # PDF documents go here
├── chroma/         # ChromaDB persistent storage
├── requirements.txt
└── README.md
```

* **chroma\_client.py**: Extracts PDF text, chunks it, stores embeddings in ChromaDB.
* **ollama\_interaction.py**: Handles embeddings and query answering with Ollama.
* **main.py**: Orchestrates the RAG workflow.

---

### Usage

1. Place PDF documents in `AI_DOCS_TEST/Reports/`.
2. Verify ChromaDB running at `localhost:8000/api/v2`.
3. Activate virtual environment (if used) and run:

```bash
python code/core/vector/main.py
```

---

### Known Issues & Fixes

1. **Ollama port in use** – Set `OLLAMA_HOST` to a free port.
2. **ModuleNotFoundError: langchain\_community** – Install `langchain-community`.
3. **MemoryError on large PDFs** – Process in smaller chunks or increase RAM.
4. **Chroma deprecation warning** – Install `langchain-chroma` and update imports.
5. **PDF directory missing** – Ensure `AI_DOCS_TEST/Reports/` exists.
6. **Segmentation fault** – Use clean virtual environment and reinstall dependencies.

---

### Notes

* Always activate the virtual environment before running scripts.
* For large datasets, use persistent storage with ChromaDB.

---

## 2. RAG UI (Avalonia Application)

### Overview

RAG UI is a cross-platform desktop and mobile application built with **Avalonia**, using **MVVM architecture** and **ReactiveUI** for reactive state management.

---

### Features

* Cross-platform UI (Windows, Linux; iOS/Android planned)
* Login window and navigation to MainWindow
* Standard message dialogs with `MessageBox.Avalonia`
* Modular structure with shared services and view models

---

### Prerequisites

* **.NET 8 SDK**
* **Visual Studio 2022/2023** with .NET Desktop Development workload
* macOS + Xcode for iOS development (optional)
* Android Studio + Emulator (optional)

---

### Project Structure

```
RAG-UI/
├─ UI/                  # Avalonia UI project
│  ├─ App.xaml           # Entry point
│  ├─ App.xaml.cs        # Initialization & lifetime
│  ├─ Views/             # XAML views
│  └─ ViewModels/        # ViewModels
├─ Services/             # Application services
├─ BusinessObjects/      # Domain/business models
└─ README.md
```

---

### Building and Running

**Windows (Desktop)**

```bash
dotnet restore
dotnet build
dotnet run --project UI
```

**iOS (Requires macOS)**

```bash
dotnet build -t:Run -f:net8.0-ios18.0 -r:iossimulator-x64
```

> Use `#if IOS` / `#if WINDOWS` to separate platform-specific logic.

---

### Multi-Targeting

```xml
<TargetFrameworks>net8.0;net8.0-ios18.0</TargetFrameworks>
```

---

### Contributing

1. Fork repository
2. Create feature branch
3. Commit changes
4. Push branch
5. Open Pull Request

---

### License

MIT License – see [LICENSE](LICENSE)
