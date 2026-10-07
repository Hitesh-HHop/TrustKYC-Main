# TrustKYC-Main
# TrustKYC — INNOBLOCK 2.0 / PS 63 (Hackathon Demo)

> ⚠️ HACKATHON-DEMO QUALITY — NOT production identity verification.
> OCR and tamper checks here are illustrative only. Do not use for real KYC.
> Synthetic documents ONLY. Never use real IDs.

## Goal
AI reads synthetic ID images, checks readability + basic tampering, cross-checks
name / document number / expiry, and records a successful result on-chain.
Approved partner apps can reuse the result without seeing or re-uploading the document.
Users can delete off-chain files and grant/revoke partner access.

## Stack
- Solidity on Sepolia testnet (no mainnet, no deploys from this agent)
- Flask + web3.py backend
- Plain HTML/CSS/JS frontend with MetaMask

## Hard rules (enforced across phases)
1. Synthetic docs only; never real IDs.
2. Read fields from image pixels using OCR. Never treat PNG metadata or
   user-entered reference fields as OCR results. If a field cannot be read, say so.
3. Only validated PASS results may be registered. On-chain = document hash + status only.
   No images or personal fields on-chain.
4. Partner queries must check wallet identity + current user-granted contract permission.
   Revocation must block later queries.
5. Documents + necessary metadata stay off-chain; support deletion + clean temp uploads.
6. Secrets in backend env vars only (`backend/.env`, never committed).
   Never expose, copy, or commit secrets from any old project archive.
7. Label OCR/tamper checks as hackathon-demo quality everywhere.

## Project layout
- `contracts/` — Solidity (Sepolia), no mainnet
- `backend/` — Flask + web3.py, env-driven, temp-file hygiene
- `frontend/` — plain HTML/CSS/JS + MetaMask, no build step
- `docs/` — demo notes, synthetic-data notice
- `storage/` — LOCAL ONLY off-chain vault (gitignored). Never commit contents.
- `storage/uploads_tmp/` — cleaned after processing
- `storage/vault/` — user docs pending explicit delete

## Phases (build status)
- Phase 1: scaffolding + safety docs + .gitignore + env template — done.
- Phase 2: Solidity contract (EIP-712 PASS attestation, partner grant/revoke) — done
  (compile/deploy in Remix is a manual operator step).
- Phase 3: backend OCR/tamper/verify endpoints — done.
- Phase 4: frontend flows (MetaMask, analyze, register, partner reuse) — done.

## Run the local demo
1. Backend (from `trustkyc\`): activate `.venv`, then `python -m backend.app`.
   Requires `backend\.env` with a **Sepolia** `SEPOLIA_RPC_URL` (not Mainnet)
   and a filled `VERIFIER_PRIVATE_KEY`; verify `GET /health` → `ok:true`.
2. Frontend: `python -m http.server 8080 --directory frontend`, then open
   `http://127.0.0.1:8080` (these are the only CORS-allowed origins).
3. In the page: connect MetaMask (Sepolia-only guard) → Check backend /health →
   paste the Sepolia contract address → analyze a synthetic fixture
   (`backend/fixtures/synthetic_valid.png`) → registerPASS → grant/revoke/query.
4. Never submit real IDs. No deploy / push / publish performed by the agent.
