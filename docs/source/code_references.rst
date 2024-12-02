Code Reference
==============

This section explains the main components of the RAG App code, including the logic for model selection, file uploading, vector database creation, and question answering.

Modules
-------

### `app.py`

#### 1. **Model Selection**
The code allows users to select a model for embeddings and question answering:

.. code-block:: python

   model_choice = st.selectbox(
       "Choose a model:",
       options=["mistral", "llama3.1", "llama2"],  
   )

   if model_choice == "llama3.1":
       embed_model = OllamaEmbeddings(model="llama3.1", base_url="http://127.0.0.1:11434")
       llm = Ollama(model="llama3.1", base_url="http://127.0.0.1:11434")
   elif model_choice == "mistral":
       embed_model = OllamaEmbeddings(model="mistral", base_url="http://127.0.0.1:11434")
       llm = Ollama(model="mistral", base_url="http://127.0.0.1:11434")
   elif model_choice == "llama2":
       embed_model = OllamaEmbeddings(model="llama2", base_url="http://127.0.0.1:11434")
       llm = Ollama(model="llama2", base_url="http://127.0.0.1:11434")

This initializes both the embedding model and the language model based on the user's selection.

#### 2. **PDF File Uploading**
The app processes uploaded PDF files to create a vector database:

.. code-block:: python

   uploaded_file = st.file_uploader("Upload a PDF file to create a new vector database", type=["pdf"])
   if uploaded_file:
       temp_file_path = os.path.join("temp", uploaded_file.name)
       os.makedirs("temp", exist_ok=True)
       with open(temp_file_path, "wb") as f:
           f.write(uploaded_file.getbuffer())

       loader = PyPDFLoader(temp_file_path)
       documents = loader.load()

This handles file uploads, stores them temporarily, and uses the `PyPDFLoader` to extract text.

#### 3. **Vector Database Creation**
The extracted text is split into chunks and embedded into a vector database:

.. code-block:: python

   text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
   docs = text_splitter.split_documents(documents)

   vector_store = Chroma.from_documents(docs, embed_model, persist_directory="./RAG/new_chroma_db")
   retriever = vector_store.as_retriever(search_kwargs={"k": 5})

This ensures the document text is preprocessed and stored in a searchable vector database.

#### 4. **Question Answering**
The app supports both retrieval-augmented generation (RAG) and fallback to a direct language model response:

.. code-block:: python

   if st.button("Get Answer"):
       if question:
           with st.spinner("Searching for the answer..."):
               if retriever:
                   docs = retriever.get_relevant_documents(question)
                   if docs:
                       response = retrieval_chain.invoke({"input": question})
                       st.write("**Answer (from context):**")
                       st.write(response["answer"])
                   else:
                       st.write("**Answer (directly from model):**")
                       response = llm(question)
                       st.write(response)
               else:
                   st.warning("No vector database found. Defaulting to model.")
                   response = llm(question)
                   st.write("**Answer (directly from model):**")
                   st.write(response)

If relevant documents are found, the app uses them to generate answers. Otherwise, it defaults to a direct language model response.

---

### Helper Functions and Utilities

#### **RecursiveCharacterTextSplitter**
This splits the document into smaller, overlapping chunks for effective retrieval and embedding.

#### **PyPDFLoader**
Used for parsing the uploaded PDF file and extracting the text content.

#### **Chroma**
Handles vector database creation, storage, and retrieval.

---

This code reference provides an explanation of key code blocks, highlighting their purpose and functionality.
