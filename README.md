# 3DEXPERIENCE Minimal Widget

A minimal 3DEXPERIENCE 3DDashboard widget that displays **Hello \<username\>** using the UWA framework and WAFData module.

## Project structure

```
index.html      — Widget entry point (UWA standalone)
js/main.js      — Application logic (fetches user, renders greeting)
README.md       — This file
```

## Prerequisites

- A **3DEXPERIENCE** platform account with dashboard access
- **Python 3** or **Node.js** (for the local web server)
- **ngrok** (to expose localhost over HTTPS)

---

## 1 — Run a local web server

Open a terminal in the project root and start a server on port **3000**.

### Option A — Python

```bash
python3 -m http.server 3000
```

> On Windows you may need `python` instead of `python3`.

### Option B — Node.js (npx)

```bash
npx serve -l 3000
```

The widget is now available at **http://localhost:3000**.

---

## 2 — Expose the server with ngrok

### Install ngrok

Download from <https://ngrok.com/download> or install via:

```bash
# macOS / Linux (snap)
snap install ngrok

# Windows (chocolatey)
choco install ngrok

# Or with npm
npm install -g ngrok
```

### Start the tunnel

```bash
ngrok http 3000
```

ngrok will display output similar to:

```
Forwarding  https://abcd-1234.ngrok-free.app -> http://localhost:3000
```

Copy the **https://…** URL — you will need it in the next step.

---

## 3 — Test in 3DEXPERIENCE

1. Log in to your **3DEXPERIENCE** platform.
2. Open the **3DDashboard**.
3. Click **+** to add a widget, then choose **Run your App** (or **Web Browser** widget).
4. Paste the **ngrok HTTPS URL** (e.g. `https://abcd-1234.ngrok-free.app`).
5. The widget should display:

   ```
   Hello <your first name>
   ```

   If the platform user name cannot be retrieved (e.g. API error or running outside the platform), it will fall back to:

   ```
   Hello User
   ```

---

## How it works

1. `index.html` loads the **UWA Standalone** framework from the Dassault CDN.
2. `js/main.js` uses `require(['DS/WAFData/WAFData'], …)` to load the WAFData module.
3. WAFData makes an authenticated GET request to `/resources/v1/modeler/person/current` to retrieve the logged-in user.
4. The user's first name is extracted from the response and displayed in the page.
