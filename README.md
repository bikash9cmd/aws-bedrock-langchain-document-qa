# AWS Bedrock Document Q&A Bot with LangChain

A Retrieval-Augmented Generation (RAG) based document question-answering system built using Amazon Bedrock, LangChain, FAISS, Amazon S3 and Amazon SageMaker.

The system processes a PDF document, converts its content into vector embeddings, retrieves the most relevant document chunks for a user query, and uses Amazon Nova Lite to generate an answer based on the retrieved context.

## Architecture

```text
PDF Document
     ↓
Amazon S3
     ↓
Amazon SageMaker / Jupyter
     ↓
PDF Loader
     ↓
Text Chunking
     ↓
Amazon Titan Embeddings
     ↓
FAISS Vector Store
     ↓
Similarity Retriever
     ↓
Relevant Document Chunks
     ↓
Amazon Nova Lite
     ↓
Final Answer
```

## How the System Works

1. The PDF document is stored in Amazon S3.
2. The document is downloaded into the SageMaker notebook environment.
3. PyPDFLoader extracts the text from the PDF.
4. LangChain splits the document into smaller overlapping chunks.
5. Amazon Titan Embeddings converts each chunk into a numerical vector representation.
6. FAISS stores the vectors and performs similarity search.
7. When a user asks a question, the retriever searches for the most relevant document chunks.
8. The retrieved context is passed to Amazon Nova Lite through Amazon Bedrock.
9. Nova Lite generates the final response using the retrieved document context.

## Technologies

* Python
* Amazon S3
* Amazon SageMaker
* Amazon Bedrock
* Amazon Titan Embeddings
* Amazon Nova Lite
* LangChain
* FAISS
* boto3
* Jupyter Notebook

## Key Concepts Demonstrated

* Retrieval-Augmented Generation (RAG)
* Semantic search
* Vector embeddings
* Vector stores
* Document chunking
* Similarity retrieval
* Foundation models
* AWS cloud-based ML/GenAI workflow

## Example

### Question

What is Lorem Ipsum?

### Retrieval

The system retrieves the three most relevant chunks from the document.

### Generation

The retrieved context is provided to Amazon Nova Lite, which generates the final answer.

## Repository Structure

```text
.
├── document_qa_rag.ipynb
├── README.md
├── requirements.txt
├── architecture.png
├── screenshots/
└── .gitignore
```

## Notes

This project was developed as a learning implementation of a RAG-based document Q&A pipeline using AWS services and LangChain.

The implementation is intended as a prototype and is not presented as a production-ready enterprise system.

## Future Improvements

* Streamlit web interface
* Multiple document support
* Persistent vector database
* Conversation history
* Source/document citation in responses
* Authentication and access control
* Production API deployment
