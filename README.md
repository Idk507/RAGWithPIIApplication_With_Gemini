Here's a detailed README file for the code you shared:

---

# RAG with PII Application

This repository contains a Retrieval-Augmented Generation (RAG) application that handles Personally Identifiable Information (PII) securely. It utilizes **LangChain**, **Presidio** for PII anonymization, and **Google's Gemini API** for generative responses. This application is designed to anonymize sensitive PII in documents, store them safely, and deanonymize them during retrieval for secure and contextually accurate responses.

## Features

- **PII Masking and Deanonymization**: Uses a custom `PIIAgent` class for anonymizing and deanonymizing sensitive information like credit card numbers, account numbers, CIF numbers, UAE phone numbers, and Emirates IDs.
- **Retrieval-Augmented Generation (RAG)**: Combines retrieval with generative AI to provide contextually relevant responses.
- **Google Gemini API Integration**: Uses Google’s generative models (e.g., Gemini) to respond to user queries based on retrieved documents.
- **Customizable Prompt Template**: Allows users to set custom prompt templates for the generative model.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Classes and Methods](#classes-and-methods)
- [Environment Variables](#environment-variables)
- [Examples](#examples)

## Installation

1. Clone the repository:
   ```bash
   git clone [<repository-url>](https://github.com/Idk507/RAGWithPIIApplication_With_Gemini)
   cd RAGWithPIIApplication_With_Gemini
   ```

2. Install required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up environment variables in a `.env` file:
   ```env
   GOOGLE_API_KEY=<Your Google Gemini API Key>
   ```

4. Load environment variables by running:
   ```python
   from dotenv import load_dotenv
   load_dotenv()
   ```
Here’s the `requirements.txt` file based on the libraries and tools used in the code:

```txt
dotenv
Chroma
HuggingFaceEmbeddings
LangChain
GoogleGenerativeAI
TextLoader
RecursiveCharacterTextSplitter
PIIAgent
PromptTemplate
FastAPI
sentence-transformers
```

Explanation of each library included in the `requirements.txt`:

1. **dotenv**: 
   - Used for loading environment variables from a `.env` file into your program. This allows sensitive data, such as API keys or other configuration settings, to be kept outside the codebase.

2. **Chroma**: 
   - A vector database used to store and retrieve document embeddings (vector representations of documents) for efficient retrieval in the Retrieval-Augmented Generation (RAG) system.

3. **HuggingFaceEmbeddings**: 
   - A class used to load pre-trained embeddings from Hugging Face models. These embeddings are typically used for turning textual content into numerical vectors.

4. **LangChain**: 
   - A framework used to develop applications powered by large language models (LLMs). LangChain allows chaining together LLMs, document retrieval, and prompt engineering.

5. **GoogleGenerativeAI**: 
   - A class used to interact with Google's Gemini API for natural language generation. It allows for easy integration with Google's powerful language model to generate text or process queries.

6. **TextLoader**: 
   - A utility that loads textual documents from a specified directory or file, enabling easy processing and storage.

7. **RecursiveCharacterTextSplitter**: 
   - A tool used to split documents into smaller chunks of text while maintaining logical coherence. It's particularly useful when dealing with large documents that need to be broken down into smaller parts for easier processing.

8. **PIIAgent**: 
   - This represents an agent responsible for handling personally identifiable information (PII). It anonymizes and deanonymizes PII from the content before storing or returning it.

9. **PromptTemplate**: 
   - A class used to define templates for prompts, especially useful in setting up input and output formats for models. It helps generate consistent and structured prompts for the system.

10. **FastAPI**: 
    - A modern, fast (high-performance), web framework for building APIs with Python 3.7+ based on standard Python type hints. It is often used for building RESTful APIs that interact with machine learning models.

11. **sentence-transformers**: 
    - A library that provides pre-trained transformer models specifically fine-tuned for generating embeddings for sentences or paragraphs. These embeddings can then be used for tasks like similarity search, clustering, or document retrieval.

### Additional Notes:
- **Chroma**: For database persistence and storage.
- **HuggingFaceEmbeddings**: To embed documents and queries as vectors.
- **LangChain and GoogleGenerativeAI**: For chaining retrieval and response generation tasks. 

Save this file as `requirements.txt` and you can install all dependencies by running:

```bash
pip install -r requirements.txt
```

Let me know if you need further details!

## Usage

To use the RAG application, follow these steps:

1. **Initialize the RAG Application**:
    ```python
    rag_app = RAGWithPIIApplication()
    ```

2. **Add a Document**:
    Add a single document with PII information, which will be anonymized before storing.
    ```python
    sample_doc = """Your document content here with potential PII"""
    rag_app.add_single_document(sample_doc)
    ```

3. **Set a Custom Prompt (Optional)**:
    ```python
    custom_prompt = """
    Context: {context}
    Question: {question}
    Please provide a professional and concise response.
    """
    rag_app.set_custom_prompt(custom_prompt)
    ```

4. **Query the System**:
    Query the application to retrieve relevant document information with PII data deanonymized.
    ```python
    query = "What is the payment amount for the attached invoice?"
    response = rag_app.query(query)
    print(f"Response: {response}")
    ```

## Classes and Methods

### `PIIAgent`
Handles the anonymization and deanonymization of PII using Presidio.

- **`mask(text: str) -> str`**: Masks PII in the provided text.
- **`unmask(anonymized_text: str) -> str`**: Deanonymizes previously masked PII.

### `RAGWithPIIApplication`
Main application class for handling documents with PII, storing them in a vector store, and querying with Google’s Gemini API.

- **`__init__(...)`**: Initializes the RAG application with the specified parameters.
- **`process_documents(documents_dir: str, batch_size: int = 100) -> None`**: Processes and stores documents from a directory.
- **`add_single_document(content: str, metadata: Dict = None) -> None`**: Adds a single document to the vector store with PII masked.
- **`query(query: str, k: int = 4, system_prompt: str = None) -> str`**: Queries the RAG system with a user question.
- **`set_custom_prompt(template: str, input_variables: List[str] = None) -> None`**: Sets a custom prompt template for queries.

## Environment Variables

To connect to the Google Gemini API, add your API key in a `.env` file in the root directory.

```env
GOOGLE_API_KEY=<Your Google Gemini API Key>
```

## Examples

Here’s an example workflow to process a document and query information:

```python
from dotenv import load_dotenv
load_dotenv()

# Initialize application
rag_app = RAGWithPIIApplication()

# Add a document
sample_doc = """
From: John Doe <john.doe@example.com>
Date: Thursday, 6 June 2024 at 12:05 PM
Subject: Invoice Payment

Attached is the invoice for payment of $50,000.
"""
rag_app.add_single_document(sample_doc)

# Set a custom prompt
custom_prompt = """
Context: {context}
Question: {question}
Provide a concise and relevant answer.
Answer:
"""
rag_app.set_custom_prompt(custom_prompt)

# Query the document
query = "What is the payment amount for the attached invoice?"
response = rag_app.query(query)
print(f"Query: {query}")
print(f"Response: {response}")
```

## Notes

- **PII Customization**: Add or modify PII patterns in the `PIIAgent` class as per your requirements.
- **Storage**: By default, documents are stored in a local Chroma vector store at `./chroma_db`.

---

This README provides a structured overview and instructions for using the RAG with PII Application, designed for developers working with sensitive data.
