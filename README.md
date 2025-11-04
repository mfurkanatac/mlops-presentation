```markdown
# FastAPI Notebook — Minimal CRUD API with Public URL

This README explains how to run **`fastAPI.ipynb`**. The notebook starts a FastAPI app (in the background), exposes it via **ngrok**, and exercises the endpoints with Python `requests`.

---

## What it does (quick map)

```

Notebook Cell 1: install deps → start FastAPI (Uvicorn) → open ngrok tunnel → print Public URL + /docs
Notebook Cell 2: call the API (POST/GET/DELETE) using requests to show it works

````

**Endpoints implemented (in-memory, no DB):**
- `GET /items` → list items
- `POST /items` → add `{id, name, price, in_stock}` (Pydantic validation)
- `DELETE /items/{item_id}` → remove by id

OpenAPI docs: `/docs` • ReDoc: `/redoc`

---

## Requirements

- Any Python 3.9+ environment that can run Jupyter notebooks
- The notebook installs its own packages in Cell 1:
  - `fastapi`, `uvicorn`, `pyngrok`, `nest_asyncio`
- For the test client cell: `requests` (already available in most envs; if not, `pip install requests`)
- (Optional) **ngrok auth token** to create a public URL
  - Put it in the notebook variable: `AUTHTOKEN = "YOUR_TOKEN_HERE"`
  - Leaving it empty attempts an anonymous tunnel (may be rate-limited)

---

## How to run (Colab or Local Jupyter)

### Option A — Google Colab
1. Upload `fastAPI.ipynb` to Colab and open it.
2. In **Cell 1**, set your ngrok token (optional):
   ```python
   AUTHTOKEN = "YOUR_TOKEN_HERE"  # or leave "" for anonymous
````

3. Run **Cell 1**. It will:

   * Install deps
   * Start Uvicorn on `0.0.0.0:8000` **in a background thread**
   * Open an ngrok tunnel and print:

     ```
     🚀 Public URL: https://xxxx-xx-xx.ngrok-free.app
     📚 OpenAPI docs: https://xxxx-xx-xx.ngrok-free.app/docs
     🔎 Redoc:       https://xxxx-xx-xx.ngrok-free.app/redoc
     ```
4. Open the printed **Public URL** → hit `/docs` to try the API interactively.
5. Run **Cell 2** to POST/GET/DELETE sample items and see responses printed.

> Stop: **Runtime → Restart runtime** (or stop the kernel). That closes the server and tunnel.

---

### Option B — Local Jupyter (VS Code / JupyterLab / classic)

1. Ensure Jupyter is installed:

   ```bash
   pip install jupyterlab  # or jupyter
   ```
2. Launch Jupyter and open `fastAPI.ipynb`.
3. In **Cell 1**, optionally set:

   ```python
   AUTHTOKEN = "YOUR_TOKEN_HERE"
   ```
4. Run **Cell 1** to start the app and get the public URL.
5. Run **Cell 2** to hit the API programmatically, or open `/docs` in your browser.

> Stop: **Interrupt/Restart kernel** in Jupyter.

---

## Try it quickly (curl)

Once Cell 1 prints `Public URL` (say it’s `https://X.ngrok-free.app`):

```bash
# Health check (implicit by docs loading)
curl -s https://X.ngrok-free.app/items

# Add items
curl -s -X POST https://X.ngrok-free.app/items \
  -H "Content-Type: application/json" \
  -d '{"id":1,"name":"Rose","price":10.5,"in_stock":true}'

# List
curl -s https://X.ngrok-free.app/items

# Delete
curl -s -X DELETE https://X.ngrok-free.app/items/1
```

---

## Notes & Troubleshooting

  * Set `AUTHTOKEN` , then re-run Cell 1.
  * Some networks block tunnels; try a different network/VPN.
* **Port already in use** may happen, you may use another port

  * Change the port in the notebook (Cell 1 → `uvicorn.run(..., port=8000)` and `ngrok.connect(8000, ...)`) to a free port (e.g., 8010).

  * The “DB” is an in-memory Python list. Restarting the kernel clears items.
* **Production warning**

  * This is a teaching/demo notebook (no auth/rate limits/DB). Don’t expose sensitive data or long-running models through this as-is.

---

## File Structure (notebook-centric)

```
fastAPI.ipynb
 ├─ Cell 1: setup + FastAPI app + endpoints + background Uvicorn + ngrok
 └─ Cell 2: demo client calls with requests (POST/GET/DELETE)
```

Enjoy building! Open `/docs` to explore the API interactively.
