# NETRA AI — Detection speed + OpenAI chat + Map upgrade

This package contains the updated static frontend and an updated Python/FastAPI backend. Keep a backup of your current files before replacing anything.

## What's improved
- Object detection runs on smaller resized frames (live: up to 320 px; manual/photo: up to 640 px) to reduce CPU work on phones.
- TensorFlow.js tries WebGL acceleration, warms the model, and uses a non-overlapping detection loop so slow frames do not pile up.
- Ask NETRA now calls the secured backend `/api/chat` route, uses OpenAI Responses API, keeps a short in-session chat history, and can include current object labels as tentative context.
- Map: tap the map to select a destination; click a nearby marker to select it for directions; added ATM, supermarket, railway station and library categories.
- Existing camera, photo upload, speech input/output, Gemini `/api/describe`, and existing map services are retained in the updated code.

## Deploy the backend on Render
1. Open your existing NETRA Python API service on Render and keep a backup of its current `main.py`.
2. Replace its `main.py` with `netra-ai-backend/main.py`. Add `netra-ai-backend/requirements.txt` as the service's requirements file (or copy its dependencies into the existing requirements file).
3. If the backend service's Root Directory is set to a folder, upload/copy these backend files into that folder. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`.
4. In Render → your backend service → Environment, set `OPENAI_API_KEY` to your own OpenAI API key. Do not put the key in `app.js`, HTML, GitHub, or a public file. Keep `GEMINI_API_KEY` if you still want the existing Gemini `/api/describe` endpoint.
5. Set `FRONTEND_ORIGIN` to the exact deployed frontend URL (scheme + host, no trailing slash), then redeploy the backend.
6. Open `https://YOUR-BACKEND.onrender.com/health`. It should show `openai_chat_configured: true` once the key is configured. Never share the key in screenshots.

## Deploy the frontend on Render
1. Copy `netra-ai-frontend/index.html`, `style.css`, `app.js`, `render.yaml` into your existing static-site repository/folder, replacing the old versions after backing them up.
2. The frontend defaults to the API URL previously used by this project: `https://netra-ai-1-i8dy.onrender.com`. If your backend URL is different, add this before `app.js` in `index.html`:
   `<script>window.NETRA_API_BASE = "https://YOUR-BACKEND.onrender.com";</script>`
3. Commit/push and redeploy the static site.

## Important
- OpenAI API calls require a valid API key and API access/billing; a ChatGPT subscription does not automatically include API usage. Usage may incur charges.
- I verified JavaScript syntax and Python syntax locally, but I cannot verify your live Render environment or API key from here.
- The first model download still needs internet and may take a little while. Resizing reduces per-frame work, but actual speed depends on the phone, browser, and network.
- Map results depend on public OpenStreetMap/Nominatim/Overpass services and can be incomplete or rate-limited. Google Maps directions open externally; NETRA does not certify a safe or accessible route.
- This is an assistive prototype, not a certified mobility or hazard-warning device.
