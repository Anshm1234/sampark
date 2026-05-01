# SAMPARK® — Smart Parking Tag System

> **Scan. Contact. Done.** — A privacy-first vehicle contact page that lets anyone reach a car owner instantly via WhatsApp or call, without ever exposing the owner's real phone number.

---

## What is Sampark?

Sampark is a QR-code-based parking tag system. A physical tag placed on a vehicle windshield contains a unique QR code. When someone scans it — because the car is blocking them, there's a scratch, a flat tyre, or any other issue — they land on a clean mobile web page where they can contact the owner through multiple channels.

No app install required. No account needed. Works on any phone camera.

---

## Features

| Feature | Description |
|---|---|
| 💬 **WhatsApp Contact** | Opens WhatsApp with a pre-filled message to the owner |
| 📞 **Call Owner** | Masked call routing — shows only last 3 digits publicly |
| 📍 **Share Location** | Sends the scanner's live GPS coordinates via WhatsApp |
| ✏️ **Leave a Note** | Type a custom message, sent directly via WhatsApp |
| 📸 **Upload Photo** | Capture or upload a photo of the parking situation |
| ⬛ **QR Code Display** | Generate a shareable QR on-screen for others to scan |
| ⚠️ **Emergency / SOS** | One-tap access to Police (112), Ambulance (108), and a registered family contact |
| 🔒 **Privacy Protection** | Phone numbers are masked — only the last 3 digits are shown publicly |

---

## How It Works

```
Physical QR Tag on Car
        │
        ▼
  Anyone scans it
        │
        ▼
  sampark.me/index.html?id=swift
        │
        ▼
  Vehicle info loads from vehicleDB
        │
        ├──► WhatsApp → opens wa.me with pre-filled message
        ├──► Call     → tel: link to masked number
        ├──► Location → Geolocation API → WhatsApp message with Google Maps link
        ├──► Note     → custom text → WhatsApp
        ├──► Photo    → captured/uploaded → WhatsApp notification
        └──► SOS      → 112 / 108 / registered emergency contact
```

Each vehicle has a unique `id` key. The QR code on the tag encodes the URL with that key, e.g. `https://sampark.me/?id=swift`.

---

## Setup & Configuration

### 1. Register Vehicles

Open `index.html` and find the `vehicleDB` object in the `<script>` section:

```javascript
const vehicleDB = {
    "swift": {
        name:      "Swift Dzire",
        plate:     "PB 10 AB 1234",
        phone:     "919463003755",   // country code + number, no +
        emergency: "917529045245",   // family/emergency contact
        icon:      "🚗"
    },
    // Add more vehicles below...
};
```

**Field reference:**

| Field | Required | Description |
|---|---|---|
| `name` | ✅ | Vehicle display name |
| `plate` | ✅ | Registration number shown on tag |
| `phone` | ✅ | Owner's WhatsApp number (with country code, no `+`) |
| `emergency` | ✅ | Emergency/family contact number |
| `icon` | Optional | Emoji shown as vehicle icon |

### 2. Generate QR Codes

For each vehicle, generate a QR code pointing to:

```
https://yourdomain.com/?id=VEHICLE_KEY
```

Example: `https://sampark.me/?id=swift`

Any free QR generator works (QR Code Monkey, QRCodeGenerator.com, etc.). Print and laminate the tag.

### 3. Deploy

This is a single self-contained HTML file — no build step, no dependencies to install. Just host it anywhere:

- **GitHub Pages** — free, just push `index.html` to a repo and enable Pages
- **Netlify / Vercel** — drag and drop the file
- **Any web host** — upload via cPanel or FTP
- **Locally** — open in a browser (note: WhatsApp links require a live server for `window.open` to work)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Vanilla HTML, CSS, JavaScript — zero frameworks |
| Fonts | Inter (UI) + JetBrains Mono (plate numbers) via Google Fonts |
| QR Generation | [qrcodejs](https://github.com/davidshimjs/qrcodejs) via CDN |
| WhatsApp | `wa.me` deep link API |
| Location | Browser Geolocation API |
| Routing | URL query parameter (`?id=`) |

---

## File Structure

```
sampark/
├── index.html        # Entire app — single file, self-contained
├── logo.png          # Optional: brand logo (falls back to "S" lettermark)
└── README.md
```

---

## Privacy & Security

- **No backend, no database** — vehicle data lives entirely in the HTML file which you control
- **Phone numbers are never shown in full** — only the last 3 digits are displayed publicly (`+91 ••••• •••755`)
- **WhatsApp routing** — contact is made through WhatsApp's own encrypted messaging; Sampark never sees message content
- **No tracking or analytics** by default — add your own if needed

> ⚠️ Because vehicle data is stored client-side in the HTML, keep your deployed file's URL unguessable or use server-side rendering if you need stronger access control.

---

## Customisation

### Changing the Colour Theme

All colours are CSS variables at the top of the `<style>` block:

```css
:root {
    --purple:   #3B0F7A;   /* Primary brand colour */
    --yellow:   #FFD600;   /* Accent — number plate, CTAs */
    --bg:       #F4F2FF;   /* Page background */
    --surface:  #FFFFFF;   /* Card backgrounds */
    --text:     #1A0050;   /* Primary text */
}
```

### Adding a Logo

Replace the `S` lettermark div in the hero with:

```html
<img src="logo.png" alt="Sampark" style="width:36px;height:36px;border-radius:11px;">
```

### Changing Emergency Numbers

Find the SOS section in the HTML and update the `href` values:

```html
<a href="tel:112" class="sos-em-card police"> <!-- Police -->
<a href="tel:108" class="sos-em-card amb">    <!-- Ambulance -->
```

---

## Browser & Device Support

| Platform | Support |
|---|---|
| Android (Chrome) | ✅ Full |
| iOS (Safari) | ✅ Full |
| Desktop browsers | ✅ Full (WhatsApp Web opens) |
| Offline | ❌ Requires internet for WhatsApp + Google Fonts |

---

## Roadmap Ideas

- [ ] Backend/database for managing vehicles without editing HTML
- [ ] Admin dashboard to view contact history and scan logs
- [ ] SMS fallback for users without WhatsApp
- [ ] Multi-language support
- [ ] Encrypted phone number storage
- [ ] Push notification to owner on scan

---

<p align="center">
  <strong>SAMPARK®</strong> — Made with ❤️ for safer, smarter parking
</p>
