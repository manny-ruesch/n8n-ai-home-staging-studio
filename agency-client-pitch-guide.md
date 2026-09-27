# B2B Agency Reseller & Client Pitch Guide: Turnkey AI Virtual Staging

## 💼 The Business Opportunity

Real estate agencies and property managers routinely pay **€25 to €35 per photo** for manual virtual staging services (BoxBrownie, Virtual Staging Lab) with 24-to-48-hour turnarounds. Physical home staging costs upwards of **€1,500 to €4,000** per property listing.

With this **n8n Micro-SaaS**, you possess a turnkey, white-label client portal that:
- Generates photorealistic virtual staging in under 30 seconds.
- Preserves room architecture, camera angles, and perspective with zero hallucinations.
- Runs directly on Google Gemini compute for **€0.13 per generation**.
- Offers 30 curated interior design styles and furniture brands (Vitra, Roche Bobois, Poliform, Scandinavian, Japandi).

You can package and sell this technology to local real estate brokerages as a premium, branded marketing portal.

---

## 💰 Recommended Pricing Models

### Model A: "The Retainer" (Recommended for Predictable MRR)
* **Setup & White-Label Customization:** €490 (One-time)
  * Custom logo, brand colors, and domain URL (e.g., `staging.agencyname.com`).
  * Creation of their dedicated Google Drive destination folder.
* **Monthly Subscription:** €149 / month
  * Includes up to 60 staging generations per month.
  * Your infrastructure cost: ~€7.80 (60 × €0.13).
  * **Your gross profit margin: 94.7% (€141.20 / mo per client).**

### Model B: "The Turnkey Listing Pack" (Per-Listing Model)
* **Per Property Pack (10 Photos):** €79
* **Your infrastructure cost:** €1.30 (10 × €0.13).
* **Profit per listing:** €77.70.

---

## 📧 High-Converting Outreach Templates

### Template 1: Cold Email to Real Estate Agency Directors

**Subject:** Interactive virtual staging portal for [Agency Name] listings

> Hi [First Name],
>
> I noticed your recent listing at [Address/Neighborhood]—great property.
>
> When marketing vacant or cluttered homes, high-end virtual staging typically lifts click-through rates by up to 40% and speeds up time-on-market. However, waiting 48 hours for third-party designers at €30/photo creates unnecessary delays.
>
> We have built an automated staging studio that delivers photorealistic transformations in 30 seconds across 30 design styles (Scandinavian, Modern Luxury, Japandi, Parisian Chic).
>
> I set up a private demo portal for your team to test with one of your raw photos:  
> 👉 [Link to your n8n Form Webhook]  
> *(Access PIN: 1234)*
>
> Would you be open to a 10-minute walkthrough this Thursday to see how this can be deployed under [Agency Name]'s own branding?
>
> Best regards,  
> [Your Name]  
> [Your Agency / Title]

---

## 🛠️ Step-by-Step Client Onboarding

1. **Brand Customization:** Open the `Page HTML` node and update the `BRAND` object:
   ```javascript
   const BRAND = {
     name: "Prestige Realty Staging",
     language: "en", // "en" | "fr" | "de"
     logoUrl: "https://yourclient.com/logo.png",
     colorDark: "#1a202c",
     colorPrimary: "#2b6cb0",
     colorAccent: "#3182ce"
   };
   ```
2. **Assign Access PIN & Quotas:** Set `ACCESS_PIN` in `Access gate` (e.g., `8492`) and set daily quota limits to enforce plan boundaries.
3. **Connect Google Drive Destination:** In `Upload Drive`, select the client's dedicated shared Google Drive folder so their team receives all high-resolution outputs automatically.
4. **Monitor Margins:** Access `https://YOUR-N8N/webhook/home-staging-stats?pin=8492` to review real-time generations, style preferences, and calculated Google API spend.
