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

## Getting Started

### Prerequisites

* An AWS account and a region where Amazon Bedrock offers both models. The notebook defaults to `us-east-1`.
* Model access enabled in the Amazon Bedrock console (**Bedrock → Model access**) for:
  * `amazon.titan-embed-text-v2:0` (Titan Text Embeddings V2)
  * `amazon.nova-lite-v1:0` (Amazon Nova Lite)
* An execution environment whose IAM role or credentials allow:
  * `s3:GetObject` on the bucket that holds your PDF
  * `bedrock:InvokeModel` on the two models above
* Python 3.10 or later. A SageMaker notebook instance with the `conda_python3` kernel works out of the box; a local Jupyter install works too if your AWS credentials are configured (for example with `aws configure` or `AWS_PROFILE`).

### Run it

1. Upload the PDF you want to query to an S3 bucket in the same region:

   ```bash
   aws s3 cp document.pdf s3://<your-bucket>/document.pdf
   ```

2. Install the dependencies (or run the install cell in section 1 of the notebook):

   ```bash
   pip install -r requirements.txt
   ```

3. Open `aws_bedrock_langchain_document_qa.ipynb` and set the values in section 3:

   | Variable    | Set it to                                  |
   |-------------|--------------------------------------------|
   | `AWS_REGION`| the region of your bucket and Bedrock models |
   | `S3_BUCKET` | your bucket name (the notebook stops with a clear error if it is left as `YOUR_S3_BUCKET_NAME`) |
   | `S3_KEY`    | the object key of your PDF                 |
   | `TOP_K`     | how many chunks to retrieve per question (default 3) |

4. Run the cells top to bottom. Section 9 prints the retrieved chunks so you can check retrieval before generation; section 11 prints the final answer.

5. Change `query` in section 12 to ask your own questions.

### Troubleshooting

| Error | Likely cause and fix |
|-------|----------------------|
| `ValueError: Set S3_BUCKET ...` | `S3_BUCKET` is still the placeholder. Set it to your bucket name in section 3. |
| `ClientError: ... (403) ... HeadObject` or `(404)` | The role lacks `s3:GetObject`, or the bucket/key is wrong. Check `S3_BUCKET` and `S3_KEY`. |
| `AccessDeniedException ... You don't have access to the model` | Model access is not enabled. Request access to both models in **Bedrock → Model access** for your region. |
| `ValidationException ... model identifier is invalid` | The model is not offered in `AWS_REGION`. Switch to a region where both models are available, such as `us-east-1`. |
| `AccessDeniedException ... bedrock:InvokeModel` | The IAM role lacks `bedrock:InvokeModel`. Add it to the role policy. |

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

![Nova Lite answering "What is Lorem Ipsum?" from the retrieved chunks](<Screenshot/Screenshot From 2026-09-27 15-53-16.png>)

More screenshots from a working run are in [`Screenshot/`](Screenshot/).

## Repository Structure

```text
.
├── aws_bedrock_langchain_document_qa.ipynb   # the full RAG pipeline, step by step
├── README.md
├── requirements.txt
├── architecture.png
├── Screenshot/                               # console and notebook output from a working run
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
