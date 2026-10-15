# GENQUANTAA Academy – Sample Website

A simple, responsive static website for the Day 6 low-code/no-code chatbot practical.

## What's included
- Homepage, sample learning tracks, learning approach, FAQ.
- Floating **demo chat widget** with scripted example responses.
- Optional official **n8n chat** widget integration when you have a live endpoint.
- No framework, package installation, API keys, or build step required.

## Important
This is a **sample classroom website**. Course descriptions are illustrative; official fees, schedules, and enrollment terms are not supplied. The fallback chatbot is **scripted**, not powered by AI. It does not store, send, or collect real student enquiries.

## Test locally
Open \`index.html\` in a browser, or serve the folder with \`python -m http.server 8000\`.

## Deploy with Vercel
1. In Vercel, choose **Add New → Project → Import Git Repository**.
2. Select \`saikirankurapti-glitch/chatbot\`.
3. Use framework **Other**, leave Build Command blank, and deploy the project root.
4. Open the generated Vercel URL. The sample website works in demo mode immediately.

## Connect the real n8n chatbot
1. In n8n, confirm your Chat Trigger + AI Agent + Gemini + Memory workflow works.
2. Set the Chat Trigger to publicly available **Embedded Chat**, then publish/activate.
3. Copy the **production** chat webhook URL from n8n (not the test URL).
4. In \`index.html\`, replace \`const N8N_CHAT_URL = "";\` with the actual HTTPS URL.
5. Commit the change. Vercel will redeploy if Git deployment is enabled.
6. In n8n, allow the website's exact Vercel origin in the Chat Trigger's CORS settings, if required.
7. Test the website chat and inspect n8n executions for successful responses.

Keep Gemini API keys and other secrets **only in n8n**, never in this repository. The public n8n Chat URL is not a secret; use rate limiting and abuse protection before deploying to real users.
