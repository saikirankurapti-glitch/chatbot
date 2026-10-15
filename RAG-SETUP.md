# Two-document n8n RAG setup (classroom demo)

## Status
The website's real n8n widget is ready on the `n8n-live-chat` branch, but **the production Chat URL has not yet been provided**. The live Vercel site on `main` is not changed by this branch.

## Sample knowledge files (fictional)
1. [Academy practice policy](sample-knowledge-base/academy-practice-policy.json)
2. [Academy course FAQ](sample-knowledge-base/academy-course-faq.json)

These are synthetic classroom examples, **not official policies or fee information**.

## Fastest n8n build
1. Open [Gemini RAG template](https://n8n.io/workflows/13575-chat-with-pdf-csv-and-json-documents-using-google-gemini-rag/) and select **Use for free**. Import into your n8n workspace.
2. Connect the Gemini chat and embeddings credentials in the appropriate nodes.
3. Configure the upload/ingestion form trigger. Test upload of the policy JSON file and confirm the vector-store insertion succeeds.
4. Upload the FAQ JSON file through the **same** ingestion path. IMPORTANT: Check the vector-store configuration and ensure a second upload **appends** to the first dataset, not clears/replaces it. Check both documents are searchable.
5. In the retrieval/chat branch, require the model to use retrieved passages only. If the information is absent, say "I cannot verify that from the uploaded documents."
6. Test questions:
   - When are practice assignments due? (policy)
   - Which example learning track includes Azure Databricks? (FAQ)
   - What is the Agentic AI course fee? (not supplied — should not invent)
   - Can a student submit an assignment late? (conditional policy)
7. For website integration, use a **Chat Trigger** node with public **Embedded Chat** and connect it to a retrieval-enabled AI Agent. The RAG template's input form/chat interface may require adapting to the Chat Trigger. A document upload trigger is **not** a website chat endpoint.
8. Set allowed origin to `https://chatbot-eight-pi-96.vercel.app`, publish the n8n workflow, and copy its **production Chat URL**.
9. Paste the URL into `chat-config.js` (`N8N_CHAT_URL`). Merge this branch into main. Vercel will redeploy the site if Git integration is enabled.
10. Check two different source questions in the website, plus the unknown-fee test, and review n8n executions.

## Demo vs production
The template uses an in-memory vector store and data may be cleared on restart. For a real public site use persistent vector storage such as Supabase, Qdrant, Pinecone or PGVector, and restrict document uploads to trusted admins. Keep all Gemini credentials in n8n, never in website JavaScript.

## Agent system instruction
You are the GENQUANTAA Academy knowledge assistant.
Use only facts from passages retrieved from the two uploaded knowledge files.
If retrieval contains no direct support, reply: "I cannot verify that from the uploaded documents."
Keep replies under 100 words and distinguish the fictional practice materials from official Academy information.
Never invent fees, schedules, course availability, names or contact details.
Do not claim that enquiries are saved or callbacks are booked.
