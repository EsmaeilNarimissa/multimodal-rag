# Multimodal Retrieval-Augmented Generation (RAG) for Polymer Science

## Overview
This repository implements a multimodal retrieval-augmented generation (RAG) pipeline tailored for polymer science documents. The pipeline combines text, tables, and images extracted from various sources to facilitate advanced document processing and retrieval, with a special focus on rheological analysis and polymer characterization.

## Key Features
- **Advanced Data Extraction**: 
  - Extracts text, tables, and images from PDF documents
  - Supports high-resolution image extraction
  - Preserves table structure with HTML formatting
  - Handles complex scientific notation and equations

- **Intelligent Chunking Strategy**: 
  - Divides content by section headings
  - Configurable chunk sizes (default: max 10,000 characters)
  - Smart text combination for small segments (<2,000 characters)
  - Creates new chunks after 6,000 characters

- **Specialized Analysis**:
  - Rheological analysis with HMMSF model support
  - Preserves key terms like extensional viscosity and shear thinning
  - Tracks quantitative relationships (η vs γ̇, Trouton ratios)
  - Maintains figure/table references for polymer flow analysis

- **Multimodal Retrieval & Summarization**:
  - Batch processing with progress tracking
  - Parallel processing of different content types
  - Error handling and validation for base64 images
  - Specialized prompts for rheological data interpretation

## Installation
Choose the installation steps based on your operating system:

### For Windows:

1. **Install Poppler for PDF Processing**:
   - Download from [Poppler for Windows](https://github.com/oschwartz10612/poppler-windows/releases/tag/v24.08.0-0)
   - Extract the ZIP file and add the `bin` folder to your system PATH
   - Minimum version: 24.08.0-0

2. **Install Tesseract for OCR**:
   - Download from [Tesseract OCR for Windows](https://github.com/UB-Mannheim/tesseract/wiki)
   - Install and add the installation path to your system PATH
   - Recommended version: 5.3.3 or higher

### For Linux:

1. **Install System Dependencies**:
   ```bash
   sudo apt-get update
   sudo apt-get install poppler-utils tesseract-ocr libmagic-dev
   ```

### For Mac:

1. **Install System Dependencies**:
   ```bash
   brew install poppler tesseract libmagic
   ```

### Common Steps for All Operating Systems:

1. **NLTK Resource Setup**:
   - Download the `averaged_perceptron_tagger_eng` ZIP file from:
     ```
     https://raw.githubusercontent.com/nltk/nltk_data/gh-pages/packages/taggers/averaged_perceptron_tagger_eng.zip
     ```
   - Unzip and place in the appropriate NLTK data directory

2. **Install Python Requirements**:
   After installing the system dependencies above, install the Python packages using the `requirements.txt` file:
   ```bash
   pip install -r requirements.txt
   ```
   
   Alternatively, you can install the packages individually:
   ```bash
   pip install -U "unstructured[all-docs]>=0.11.0" 
   pip install pillow>=10.0.0 lxml>=4.9.0 
   pip install langchain>=0.1.0 langchain-community>=0.0.10 
   pip install langchain-openai>=0.0.5 langchain-groq>=0.0.1 
   pip install python-dotenv chromadb tiktoken
   ```

3. **API Keys Setup**:
   Create a `.env` file with the following keys:
   ```
   OPENAI_API_KEY=your_openai_key
   GROQ_API_KEY=your_groq_key
   LANGCHAIN_API_KEY=your_langchain_key
   LANGCHAIN_TRACING=true
   LANGCHAIN_PROJECT=your_langchain_project
   LANGCHAIN_ENDPOINT=your_langchain_endpoint
   ```

> **Note**: For detailed installation instructions, dependency versions, and troubleshooting tips, please refer to the "3.0 Overview" section in the `MM_RAG_Polymer-Science.ipynb` notebook.

## Usage
1. **Data Loading & Processing**:
   ```python
   # Load and process PDF documents
   chunks = partition_pdf(
       filename="your_file.pdf",
       infer_table_structure=True,
       strategy="hi_res",
       extract_image_block_types=["Image", "Table"],
       chunking_strategy="by_title"
   )
   ```

2. **Content Inspection**:
   - Use `inspect_all_elements()` to view extracted images and tables
   - Verify base64 image encoding with provided validation tools
   - Review table HTML structure and formatting

3. **Summarization**:
   ```python
   summaries = summarize_multimodal(chunks)
   # Access different content types
   text_summaries = summaries["texts"]
   table_summaries = summaries["tables"]
   image_summaries = summaries["images"]
   ```

4. **Retrieval Testing**:
   - Test the retrieval system with polymer science queries
   - Review retrieved documents and their relevance scores
   - Analyze multimodal content connections

## Error Handling
- Graceful handling of PDF processing errors
- Base64 image validation and error reporting
- Batch processing with error recovery
- Progress tracking with detailed feedback

## References
- [Multi-Vector Retrieval Documentation](https://python.langchain.com/docs/how_to/multi_vector/)
- [LangChain Semi Structured Multimodal RAG Cookbook](https://github.com/langchain-ai/langchain/blob/master/cookbook/Semi_structured_and_multi_modal_RAG.ipynb)
- [Blog Post about RAG with LangChain](https://blog.langchain.dev/semi-structured-multi-modal-rag/)
- [How to load PDFs with LangChain](https://python.langchain.com/docs/how_to/document_loader_pdf/)
- [Parent Document Retriever Documentation](https://python.langchain.com/api_reference/langchain/retrievers/langchain.retrievers.parent_document_retriever.ParentDocumentRetriever.html/)
