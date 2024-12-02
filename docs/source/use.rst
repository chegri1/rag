use
=====

usage
-----

The RAG App is designed to answer user questions by retrieving relevant information from uploaded documents or falling back to a language model.

1. **Open the app**:
   Run the app and open the Streamlit interface in your browser.

2. **Choose a model**:
   Select a model from the dropdown menu:
   - mistral
   - llama3.1
   - llama2

3. **Upload a document**:
   Upload a PDF file to create a vector database. The app will:
   - Split the document into smaller chunks.
   - Embed the chunks into a vector database.

4. **Ask questions**:
   Type your question into the input box. The app will:
   - Search the vector database for relevant documents.
   - If no relevant documents are found, fall back to the selected model.

5. **View the answer**:
   The answer will be displayed based on the retrieved context or generated directly by the model.

Error Handling
--------------

- If no PDF file is uploaded, the app will default to querying the language model.
- Any file processing errors will be displayed in the interface.
