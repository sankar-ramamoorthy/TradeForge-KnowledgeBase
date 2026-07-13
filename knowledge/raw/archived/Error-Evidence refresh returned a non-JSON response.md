
ok i think some setup is still required. Evidence refresh returned a non-JSON response. Check the local dev proxy and API route configuration.

Vite is returning the frontend HTML instead of proxying the new API route

A “non-JSON response” commonly means the request expected JSON but received index.html from Vite.

Verify the EV route directly

Open:

http://127.0.0.1:8000/docs

Search the Swagger page for terms such as:

evidence
refresh
watchlist
attention

Yes—this sounds more like a local runtime/proxy mismatch than an EV implementation failure.

The two most likely causes are:

## 1. The backend container is still running the pre-EV code

Your FastAPI command does **not** use `--reload`, so pulling or merging new Python files does not restart Uvicorn automatically.

From the runtime repo:

```powershell
cd C:\Users\bosto\dockerstuff\TradeForge-Project\TradeForge
docker compose restart tradeforge
```

Then refresh the browser and try Evidence Refresh again.

Because EV may have introduced dependencies or startup wiring, the safer reset is:

```powershell
docker compose down
docker compose up --build
```

Keep the frontend running separately with:

```powershell
cd C:\Users\bosto\dockerstuff\TradeForge-Project\TradeForge\frontend
npm run dev
```

## 2. Vite is returning the frontend HTML instead of proxying the new API route

A “non-JSON response” commonly means the request expected JSON but received `index.html` from Vite.

Open Firefox Developer Tools:

```text
F12 → Network → click Evidence Refresh
```

Inspect the failed request and note:

```text
Request URL
Status
Response Content-Type
Response body
```

Interpretation:

```text
text/html
→ Vite handled the request instead of proxying it

application/json with 404
→ backend is running, but the route/path is missing or stale

502 / proxy error
→ Vite cannot reach FastAPI at port 8000

500 application/json
→ backend route ran but failed internally
```

## Verify the EV route directly

Open:

```text
http://127.0.0.1:8000/docs
```

Search the Swagger page for terms such as:

```text
evidence
refresh
watchlist
attention
```

If the new route is absent, restart/rebuild the backend.

If it appears in Swagger, use **Try it out** there. That bypasses Vite completely:

* Works in Swagger but fails in TradeForge UI → frontend proxy or frontend URL problem.
* Fails in Swagger too → backend route/configuration problem.

## Check the Vite proxy

Look at:

```text
frontend\vite.config.ts
```

The safest development proxy usually forwards all API-like paths, or at least every backend prefix. For example:

```ts
server: {
  host: "127.0.0.1",
  proxy: {
    "/health": "http://127.0.0.1:8000",
    "/session": "http://127.0.0.1:8000",
    "/lifecycle": "http://127.0.0.1:8000",
    "/workspaces": "http://127.0.0.1:8000",
    "/market": "http://127.0.0.1:8000",
    "/evidence": "http://127.0.0.1:8000",
    "/watchlist": "http://127.0.0.1:8000",
  },
}
```

The actual prefixes must match the routes shown in Swagger. After changing `vite.config.ts`, stop and restart:

```powershell
Ctrl+C
npm run dev
```

Start with `docker compose down` followed by `docker compose up --build`. Given that M-EV was just merged and your backend has no reload flag, stale backend code is the first thing I would eliminate.


Good — this confirms the backend is current and the EV routes are registered.

The key route exists:

```text
POST /evidence/refresh/run
```

So the problem is now most likely in the frontend dev proxy.

The error message suggests the frontend called `/evidence/refresh/run`, but Vite returned HTML instead of forwarding the request to FastAPI.

Open:

```text
C:\Users\bosto\dockerstuff\TradeForge-Project\TradeForge\frontend\vite.config.ts
```

Check whether `/evidence` is included in the proxy configuration. It probably has older prefixes such as `/health`, `/lifecycle`, `/workspaces`, and `/market`, but not the newly added `/evidence`.

Add:

```ts
"/evidence": {
  target: "http://127.0.0.1:8000",
  changeOrigin: true,
},
```

For example:

```ts
server: {
  host: "127.0.0.1",
  proxy: {
    "/health": "http://127.0.0.1:8000",
    "/session": "http://127.0.0.1:8000",
    "/lifecycle": "http://127.0.0.1:8000",
    "/workspaces": "http://127.0.0.1:8000",
    "/market": "http://127.0.0.1:8000",
    "/evidence": "http://127.0.0.1:8000",
  },
},
```

Then restart Vite because proxy configuration is only read at startup:

```powershell
Ctrl+C
npm run dev
```

After restarting, reload:

```text
http://127.0.0.1:5173
```

and try Evidence Refresh again.

You can also verify the backend independently in Swagger:

```text
POST /evidence/refresh/run
→ Try it out
→ Execute
```

Because the endpoint is visible in Swagger, Docker and the route refactor are not the problem. The missing `/evidence` proxy entry is the most probable cause.


PS C:\Users\bosto\dockerstuff\TradeForge-Project\TradeForge> type C:\Users\bosto\dockerstuff\TradeForge-Project\TradeForge\frontend\vite.config.ts
import react from "@vitejs/plugin-react";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
    strictPort: false,
    proxy: {
      "/health": "http://127.0.0.1:8000",
      "/session": "http://127.0.0.1:8000",
      "/workspaces": {
        target: "http://127.0.0.1:8000",
        bypass: (request) => {
          return request.headers.accept?.includes("text/html")
            ? "/index.html"
            : undefined;
        }
      },
      "/lifecycle": "http://127.0.0.1:8000",
      "/replay": "http://127.0.0.1:8000",
      "/advisory": "http://127.0.0.1:8000",
      "/provider-governance": "http://127.0.0.1:8000",
      "/admin": "http://127.0.0.1:8000"
    }
  }
});
PS C:\Users\bosto\dockerstuff\TradeForge-Project\TradeForge>

That confirms it: the Vite proxy is missing the new `/evidence` route prefix.

It is also missing `/market`, even though TradeForge has `/market/snapshots`.

Update `vite.config.ts` to include both:

```ts
import react from "@vitejs/plugin-react";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
    strictPort: false,
    proxy: {
      "/health": "http://127.0.0.1:8000",
      "/session": "http://127.0.0.1:8000",
      "/workspaces": {
        target: "http://127.0.0.1:8000",
        bypass: (request) => {
          return request.headers.accept?.includes("text/html")
            ? "/index.html"
            : undefined;
        },
      },
      "/lifecycle": "http://127.0.0.1:8000",
      "/replay": "http://127.0.0.1:8000",
      "/advisory": "http://127.0.0.1:8000",
      "/market": "http://127.0.0.1:8000",
      "/evidence": "http://127.0.0.1:8000",
      "/provider-governance": "http://127.0.0.1:8000",
      "/admin": "http://127.0.0.1:8000",
    },
  },
});
```

Then restart the frontend server:

```powershell
# In the PowerShell window running Vite
Ctrl+C
npm run dev
```

Reload:

```text
http://127.0.0.1:5173
```

Then run Evidence Refresh again.

The failure makes sense now:

```text
Frontend calls POST /evidence/refresh/run
→ Vite has no /evidence proxy rule
→ Vite returns frontend HTML
→ frontend expects JSON
→ “Evidence refresh returned a non-JSON response”
```

This should probably be recorded as a small regression issue because EV introduced a new API prefix without extending the development proxy configuration.
