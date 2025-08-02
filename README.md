def main():
    st.set_page_config(page_title="🤖 Hybrid Chatbot", page_icon="📄")
    st.title("🤖 Chat with AI (Groq + RAG)")
    st.caption("Ask anything! Upload a PDF to answer from your own docs.")

    llm=load_llm()
    chain=build_chain(llm)

    if "history" not in st.session_state:
        st.session_state.history=[]

    uploaded_file=st.file_uploader("📄 Upload PDF for RAG", type="pdf")
    retriever=None

    if uploaded_file:
        with st.spinner("📄 Processing PDF..."):
            with tempfile.NamedTemporaryFile(delete=False, suffix=".pdf") as tmp:
                tmp.write(uploaded_file.read())
                tmp_path=tmp.name

            loader=PyPDFLoader(tmp_path)
            docs=loader.load()

            splitter=RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=100)
            chunks=splitter.split_documents(docs)

            embeddings=HuggingFaceEmbeddings()
            vectorstore=FAISS.from_documents(chunks, embeddings)
            retriever=vectorstore.as_retriever(search_type="similarity", k=3)

        st.success("✅ PDF processed and ready for questions!")

    for entry in st.session_state.history:
        with st.chat_message(entry["role"]):
            st.markdown(entry["content"])

    user_input=st.chat_input("Ask your question:")

    if user_input:
        st.chat_message("user").markdown(user_input)

        full_prompt=build_context_prompt(user_input, retriever)

        with st.spinner("💬 Thinking..."):
            response=chat_with_bot(chain, full_prompt)

        st.chat_message("ai").markdown(response)

        st.session_state.history.append({"role": "user", "content": user_input})
        st.session_state.history.append({"role": "ai", "content": response})

    if st.button("🗑️ Clear Chat"):
        st.session_state.history=[]
        st.rerun()
