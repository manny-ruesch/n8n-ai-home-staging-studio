# AI Virtual Home Staging Micro-SaaS & Agency Kit (n8n + Gemini)

A turnkey, production-grade **n8n automation workflow** that serves a complete, responsive **White-Label Web Application** directly via webhooks. Enables real estate agencies, photographers, and automation consultants to generate photorealistic interior staging in under 30 seconds for **~€0.13 per generation** directly on Google Gemini compute.

---

## 🏗️ Architecture & Serverless Pipeline Flow

```
┌─────────────────────────────────┐
│     Client Browser (SPA)        │ (Vanilla JS, Canvas compression, before/after slider)
└─────────────────────────────────┘
         │
         ├───► GET /webhook/home-staging-form (Serves complete branded HTML/CSS/JS)
         ├───► POST /webhook/home-staging-submit (Honeypot, Access PIN, Quota Gate)
         ├───► GET /webhook/home-staging-status?jobId=... (Polling every 4 seconds)
         ├───► GET /webhook/home-staging-image?jobId=... (Streams final high-res render)
         └───► GET /webhook/home-staging-stats?pin=... (Analytics & Cost Dashboard)
         │
         ▼
┌─────────────────────────────────┐
│   Access & Quota Security Gate  │───► Honeypot check, MIME verification, PIN gate,
└─────────────────────────────────┘     Daily usage quota checked against n8n DataTable
         │
         ▼
┌─────────────────────────────────┐
│       Brain Logic Node          │───► Room detection, Action routing (Furnish/Empty/Clean/Upscale),
└─────────────────────────────────┘     Brand-safety shield & Strict Camera-Lock rules
         │
    ┌────┴────────────────────────┐
    ▼                             ▼
┌───────────────────────┐   ┌───────────────────────┐
│ Gemini Nano Banana    │   │ Optional 4K Upscaler  │
│ Image-to-Image Core   │   │ (fal.ai ESRGAN)       │
└───────────────────────┘   └───────────────────────┘
    └────┬────────────────────────┘
         │
         ▼
┌─────────────────────────────────┐
│      Google Drive Storage       │───► Saves high-res output: {action}_{room}_{timestamp}.jpg
└─────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────┐
│    DataTable Status Resolution  │───► Marks job "done", emits driveFileId, serves client
└─────────────────────────────────┘
```

---

## 🔑 Credential Configuration Matrix

| Integration | Target Nodes | Required Credentials |
| :--- | :--- | :--- |
| **Google Gemini** | `Edit image` | Google Gemini API Key (`models/gemini-3-pro-image-preview` / Nano Banana Pro) |
| **Google Drive** | `Upload Drive`, `Download` | Google Drive OAuth2 credential with file write/read permissions |
| **n8n DataTables** | `Count today`, `Reserve job`, `Mark done`, `Mark error` | Built-in n8n DataTables engine (Zero external setup required) |
| **Optional 4K Upscale** | `Fal esrgan` | Header Auth: `Authorization: Key YOUR_FAL_KEY` (fal.ai) |

---

## ⚡ Quickstart Setup Guide (Under 10 Minutes)

### Step 1: Import Workflow into n8n
1. In your n8n workspace, navigate to **Workflows** > **Add workflow** > **Import from File**.
2. Select `workflow_n8n.json`.

### Step 2: Initialize Database Table
1. Find the **Setup: run once** node (bottom left of the canvas).
2. Click **Execute step** once.
3. This automatically initializes the internal `staging_jobs` data table with all required schema columns (`jobId`, `status`, `driveFileId`, `action`, `room`, `style`, `day`, `errorMessage`).

### Step 3: Connect Credentials
1. Open the `Edit image` node and assign your **Google Gemini** credentials.
2. Open the `Upload Drive` and `Download` nodes and assign your **Google Drive OAuth2** credentials.
3. Choose a destination folder in `Upload Drive` for completed staging renders.

### Step 4: Rebrand & Deploy
1. Open the `Page HTML` node and customize the `BRAND` object at the top:
   - `name`: Your agency or client name.
   - `language`: `"en"`, `"fr"`, or `"de"`.
   - `logoUrl`: Link to your hosted PNG logo.
   - `colorPrimary` & `colorAccent`: Custom brand hex colors.
2. Toggle the workflow to **Active**.
3. Access your live web application at:  
   `https://YOUR-N8N-DOMAIN/webhook/home-staging-form`

---

## 🎨 30 Built-In Design Styles & Architectural Brand Safety

The workflow includes a prompt engineering library featuring 30 distinct interior design codes:
* **Signature Agency Style:** Completely customizable prompt code in the `Brain` node to enforce your own firm's aesthetic.
* **Classic & Architectural Styles:** Modern, Luxury Modern, Ultra Luxury, Scandinavian, Industrial, Mid-Century, Art Deco, Parisian Chic, Wabi-Sabi, Eclectic Chic.
* **Trendy Aesthetics:** Japandi, Nature Inspired, Boho Chic, Coastal.
* **Grand Scale Spaces:** American Luxury, Hotel Lounge, California Modern.
* **Curated Designer Brands:** Vitra, BoConcept, Roche Bobois, Minotti, Knoll, Herman Miller, Ligne Roset.
* **Kitchen & Bath Specialists:** Bulthaup, Boffi, Arthur Bonnet, Poliform, Porcelanosa.

### The Brand-Safety Shield
The `Brain` node enforces strict room compatibility: kitchen brands (Bulthaup, Boffi) are blocked in living rooms; bathroom brands (Porcelanosa) are restricted to bathrooms. If an incompatible selection is made, the engine safely falls back to *Luxury Modern* without failing the generation.

### Strict Anti-Hallucination Camera Lock
Every prompt is terminated with frozen geometry constraints: camera position, focal length, wall positions, ceiling moldings, and electrical outlets are locked in-place to prevent architectural distortion.

---

## 📦 What's Included in this Distribution

- `workflow_n8n.json`: Complete 46-node production workflow with embedded SPA, polling engine, and cost analytics.
- `agency-client-pitch-guide.md`: Comprehensive B2B reseller guide with pricing models, client proposals, and cold outreach scripts.
- `README.md`: Technical specifications, credential setup, and deployment documentation.
- `ai-virtual-staging-agency-kit.zip`: Complete packaged bundle.

---

## 🛡️ Production Security Checklist

* **Access PIN Protection:** For public deployment, set an `ACCESS_PIN` in the `Access gate` node.
* **Budget Cap:** Set `DAILY_LIMIT` in the `Access gate` node (default: 30) to protect your API budget.
* **Rate Limiting:** For high-volume production, place n8n behind a reverse proxy (Cloudflare or Nginx) with IP-based rate limiting on `/webhook/home-staging-submit`.
