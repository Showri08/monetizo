# Monetizo – Landing Page

Single-file static site (`index.html`). No build step.

## 1. Customize
Open `index.html` and edit the `CONFIG`, `SERVICES` and `BUNDLES` blocks near the bottom:
- brand name, contact line, payment numbers
- service names, descriptions, features and prices

## 2. Connect the order form (Google Sheet, free)
1. Create a Google Sheet. First row headers:
   `submitted_at, service_name, service_price, full_name, email, phone, profile_link, payment_method, sender_account, amount_paid, transaction_id, notes`
2. In the sheet: **Extensions → Apps Script**, paste:

```js
function doPost(e) {
  const sh = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const p = e.parameter;
  sh.appendRow([
    p.submitted_at, p.service_name, p.service_price, p.full_name, p.email,
    p.phone, p.profile_link, p.payment_method, p.sender_account,
    p.amount_paid, p.transaction_id, p.notes
  ]);
  return ContentService.createTextOutput("ok");
}
```
3. **Deploy → New deployment → Web app**, execute as *Me*, access *Anyone*. Copy the URL.
4. Paste it into `formEndpoint` in `index.html`.

(Alternative: create a free Formspree form and paste its endpoint URL.)

## 3. Deploy to Vercel
**Option A – CLI**
```bash
npm i -g vercel
cd <this-folder>
vercel --prod
```
When asked for the project name, enter `monetizo` (or your chosen brand name) so the URL becomes `monetizo.vercel.app`.

**Option B – GitHub**
Push the folder to GitHub → vercel.com/new → import → set the project name to your brand name → Deploy.

If the name is taken, Vercel will ask for another. Pick a close variation or connect a custom domain.

## Security notes
- The form deliberately does **not** collect card numbers, CVV, PINs or one-time codes.
- Keep the Google Sheet private, since it holds customers' personal data.
- Always verify the transaction ID in your Zelle/Cash App/Venmo/PayPal/bank app before starting work.
