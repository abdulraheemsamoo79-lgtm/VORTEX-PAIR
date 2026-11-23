# VORTEX MD — Pairing Portal

VEXORA wale portal system ki tarah ka central pairing portal:

- **Pair page (`index.html`)** — servers ki list, har server ke live users + limit,
  server chun kar number dalne par pairing code (code bot server par banta hai,
  portal sirf proxy karta hai: `/api/proxy` → bot ka `/active` + `/api/code`).
- **Admin panel (`/admin`)** — login ke baad servers add / edit / delete
  (naam, Railway URL, user limit). Login `ADMIN_USER` / `ADMIN_PASS`
  Vercel env vars se ata hai; session ek signed httpOnly cookie mein rehta hai.

## Deploy (Vercel)

1. Is repo ko Vercel par import karo.
2. Project → Storage → **KV (Upstash)** store bana kar project se connect karo
   (admin panel se servers save karne ke liye zaroori hai — is ke baghair
   list sirf `data/servers.json` wali rehti hai).
3. Project → Settings → Environment Variables:
   - `ADMIN_USER` — admin ka username
   - `ADMIN_PASS` — admin ka password
4. Deploy. Phir `/admin` par login karke SERVER 01 ka URL apne
   Railway bot wale domain se badal do.

## Bot side

Har bot server (VORTEX-MD repo) ye endpoints deta hai:

- `GET /active` → `{ active, count, limit, server, serverId }`
- `GET /api/code?number=<number>` → `{ success, code }`
