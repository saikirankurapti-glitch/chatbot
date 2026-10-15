# Prompt for n8n AI Workflow Builder — Two-file RAG website chatbot

Paste **only the builder prompt below** into n8n AI Workflow Builder. The current sample documents are public, fictional demo content on GitHub.

## Builder prompt

Create a NEW n8n workflow named **GENQUANTAA Academy — 2 Document RAG Website Chatbot**. Use built-in n8n nodes and Google Gemini; don't use external code services or paid vector DBs for this demo.

GOAL:
- A public n8n Chat Trigger receives visitor questions from the embedded n8n chatbot on https://chatbot-eight-pi-96.vercel.app/
- An AI Agent retrieves relevant passages from TWO knowledge-base documents and answers only when those passages support the answer.
- An initialization branch ingests both GitHub documents into a single shared vector store.
- The chat branch does NOT ask users to upload files. Only the workflow administrator can load/refresh the knowledge base.
- This is a DEMO using fictional source documents. Never call the sources official Academy policies.

SOURCE DOCUMENT 1:
https://raw.githubusercontent.com/saikirankurapti-glitch/chatbot/n8n-live-chat/sample-knowledge-base/academy-practice-policy.json

SOURCE DOCUMENT 2:
https://raw.githubusercontent.com/saikirankurapti-glitch/chatbot/n8n-live-chat/sample-knowledge-base/academy-course-faq.json

Create TWO branches inside ONE workflow:

BRANCH A — ADMIN KNOWLEDGE INDEXING
1. Manual Trigger (not available to public website visitors).
2. HTTP Request node "Get Practice Policy" downloads Source Document 1 as text.
3. HTTP Request node "Get Course FAQ" downloads Source Document 2 as text.
4. Normalize the responses so each document remains readable natural-language text, preserving all source facts and its filename/title as metadata. Since these are JSON knowledge files, avoid embedding a single huge JSON object without extracting usable text. Use Merge (append) or an equivalent no-code mapping to pass BOTH documents.
5. Use a Default Data Loader, Recursive Character Text Splitter (approx. 800 characters/chunk and 100-character overlap), Google Gemini Embeddings and Simple Vector Store INSERT DOCUMENTS.
6. Set the Simple Vector Store **Memory Key** to EXACTLY genquantaa_academy_demo_kb_v1.
7. If a clear-store/reset option exists, do not clear the store between document 1 and document 2. For a full rebuild, reset once BEFORE inserting either file, then append both.
8. Save the source title/filename in chunk metadata, so the agent can mention which document supports an answer.
9. Include a way to inspect the stored documents or verify one fact from each source after indexing. Never claim ingestion succeeded before execution confirms it.

BRANCH B — PUBLIC WEBSITE CHAT
1. Chat Trigger named "Website Chat", with public chat enabled, **Embedded Chat** mode, and production Chat URL available after publishing.
2. Set Allowed Origins/CORS to the EXACT website origin: https://chatbot-eight-pi-96.vercel.app (no trailing slash).
3. Connect Chat Trigger to an AI Agent.
4. Connect a Google Gemini Chat Model to the AI Agent.
5. Connect Simple Memory for per-session conversational context. Never share memory across unrelated chat sessions.
6. Add a retrieval tool for the AI Agent using Simple Vector Store "Retrieve Documents (As Tool for AI Agent)", or a supported Vector Store Question Answer Tool. Connect Gemini Embeddings and configure the SAME Memory Key: genquantaa_academy_demo_kb_v1. Name the retrieval tool academy_knowledge_search and describe it as the source for Academy policy and course FAQ questions.
7. Configure AI Agent to call the retrieval tool before answering any Academy-related factual question. Set a sensible top-K (for example, 4).
8. Return the AI Agent's text reply directly to the Chat Trigger, with a short, friendly website style. Do not show internal raw JSON to visitors.
9. Do not add any public file-upload endpoint, email sender, CRM, callback booking, payments or student data persistence.

AI AGENT SYSTEM INSTRUCTIONS:
"You are the GENQUANTAA Academy knowledge-base demo assistant. Your ONLY factual reference for Academy questions is academy_knowledge_search, containing two fictional classroom documents. Always retrieve evidence before answering Academy questions. If no relevant passage clearly supports an answer, say exactly: 'I cannot verify that from the uploaded documents.' Never invent course fees, dates, schedules, certificate rules, policies or guarantees. When answering from the sample policy or FAQ, make clear it is fictional demonstration content and cite the relevant source document title/section if retrieved. Keep replies under 100 words. Never claim a student enquiry has been saved or a callback has been booked. If the user asks for a text summary or action list, use only text supplied in their message and label that as based on user-provided text."

TEST QUESTIONS AFTER BOTH DOCUMENTS ARE INDEXED:
1. "When are practice assignments due?" — should retrieve fictional practice policy.
2. "Which course includes Azure Databricks?" — should retrieve fictional course FAQ.
3. "Can assignments be submitted late?" — include the approval condition.
4. "What is the Agentic AI course fee?" — unavailable; don't invent a fee.
5. "What is the official certification fee?" — unavailable; don't invent.

FINAL CHECKS:
- Both indexing and chat branches are connected with correct resource modes and credential mappings.
- The indexing branch must be run successfully at least once BEFORE testing the website chatbot.
- Check Gemini chat and embedding credentials; identify any unfinished manual settings.
- Explain how to publish the workflow, copy the actual PRODUCTION Chat URL from Website Chat, and test the public embedded endpoint.
- Do not pretend to publish or run anything unless execution actually succeeded.
- This uses n8n's in-memory Simple Vector Store for DEMO only; its contents may be lost on restart. For a live production Academy website, replace it with persistent storage (e.g. Supabase, Qdrant or PGVector), restrict ingestion, and add abuse protection.

Generate the complete workflow now.

## Connect the website

The integration code is prepared in `index.html` and a separate `chat-config.js` on the GitHub `n8n-live-chat` branch.

After n8n publishes the workflow and shows the REAL production Chat URL:
1. Put that URL in `chat-config.js`.
2. Merge `n8n-live-chat` into `main`.
3. Vercel redeploys the site if automatic Git deployments are enabled.
4. Test source questions and unsupported questions on https://chatbot-eight-pi-96.vercel.app/

**Never put a Gemini API key in GitHub or frontend JavaScript.**
