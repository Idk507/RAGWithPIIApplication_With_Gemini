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
