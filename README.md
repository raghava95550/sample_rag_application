# Sample RAG Application

A notebook-based retrieval-augmented generation (RAG) application that answers questions using information from local PDF, Word, and text documents. It loads and splits the documents, indexes their text as vectors, retrieves relevant passages for each question, and asks a language model to compose a concise answer grounded in those passages.

## Models

- **Embedding model:** OpenAI `text-embedding-3-small`. It converts document chunks and search queries into vectors for semantic similarity search.
- **Chat model (LLM):** OpenAI `gpt-4o-mini`. It answers questions using the relevant text retrieved from the document collection.

The embeddings support indexing and retrieval; the prompt receives the retrieved document text, not the embedding vectors themselves.

## How it works

1. Loads `.pdf`, `.docx`, and `.txt` files recursively from the configured document folder(s).
2. Splits loaded document text into chunks of up to 1,000 characters, with 200 characters of overlap.
3. Creates a Chroma vector store from the chunks using `text-embedding-3-small`.
4. Retrieves relevant chunks for a question and formats their text as context.
5. Sends the context and question to `gpt-4o-mini`. The prompt asks for a concise answer of at most three sentences and to say when the answer is not known.

## Requirements

- Python and Jupyter Notebook
- An OpenAI API key with access to the models above
- Packages listed in [`requirements.txt`](./requirements.txt)

Install the dependencies in your Python environment:

```bash
pip install -r requirements.txt
```

## Configuration and usage

1. Create a `.env` file in the project directory and add your API key:

   ```dotenv
   OPEN_API_KEY=your_openai_api_key
   ```

   The notebook loads this value and assigns it to `OPENAI_API_KEY` for the OpenAI client. Keep the key private; do not commit `.env`.

2. Open `main.ipynb` and update the `folders` list to point to the directory containing your documents. The example path in the notebook is machine-specific.
3. Run the notebook cells in order to load and index the documents and create the RAG chain.
4. Ask a question by invoking the chain with a question string:

   ```python
   rag_chain.invoke({"question": "What experience does Raghavendra have?"})
   ```

The notebook builds the Chroma vector store from the documents when run; the index is not configured as a persistent store.

