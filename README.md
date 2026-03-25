# 🎵 Mii-Music — DJ Song Request App

A real-time song and karaoke request platform for DJs and live event hosts. Fans scan a QR code, submit requests from their phone, and they appear instantly on the DJ's admin panel — no app download required.

---

## ✨ Features

### For Fans
- Scan a QR code to open the request page instantly on any phone
- Choose between **Song Request** or **Karaoke Request**
- Simple, fast form — song name, artist, and your name
- Karaoke requests include a performer name field
- Branded with venue logo and event name
- Works on any device, any browser — no app needed

### For the DJ / Host
- **Live request board** — requests appear in real time the moment a fan submits
- **Approval flow** — review every request before it appears on the display screen. Pending requests stay hidden from the audience until you approve them
- **Track info + cover art** — on approval, automatically searches MusicBrainz for the track and displays album artwork on the request card and display screen
- **Spotify integration** — connect Spotify to get a one-click button that opens the exact track in Spotify, ready to play or queue manually
- **Filter requests** by type: All / Pending / Approved / Songs / Karaoke
- Mark requests as **Now Playing**, **Played**, or **Remove**
- **QR code generator** — customise with DJ name, event and venue, generate and share instantly
- **Auto-refresh** every 60 seconds as a backup to the real-time sync
- **Display screen** — clean full-screen view showing Now Playing + approved queue with album artwork, designed for a TV or second monitor behind the DJ booth

---

## 🛠️ Built With

| Technology | Purpose |
|---|---|
| **HTML / CSS / JavaScript** | Frontend — no framework, runs in any browser |
| **Supabase** | Real-time database and live sync across all devices |
| **MusicBrainz API** | Track search and metadata — free, no API key required |
| **Cover Art Archive** | Album artwork — free, no API key required |
| **Spotify Web API (PKCE)** | One-click open in Spotify (see note below) |
| **QRCode.js** | Client-side QR code generation |
| **GitHub Pages** | Free static hosting |
| **Electron** *(optional)* | Package as an installable desktop app |

---

## 📁 Files

| File | Description |
|---|---|
| `index.html` | Fan-facing request page — what fans see when they scan the QR code |
| `admin.html` | DJ admin panel — request board, QR generator, display screen, Spotify connect |

---

## 🚀 Setup

### 1. Supabase
Create a free project at [supabase.com](https://supabase.com) and run this SQL:

```sql
create table requests (
  id bigint generated always as identity primary key,
  song text not null,
  artist text,
  fan_name text,
  status text default 'pending',
  type text default 'music',
  performer text,
  created_at timestamptz default now()
);

alter table requests enable row level security;
create policy "Anyone can insert" on requests for insert with check (true);
create policy "Anyone can read" on requests for select using (true);
create policy "Anyone can update" on requests for update using (true);
```

### 2. Spotify (optional)
Connecting Spotify adds a one-click **Open in Spotify** button to each approved request card, so you can jump straight to the track without searching manually.

- Create an app at [developer.spotify.com](https://developer.spotify.com)
- Add your redirect URI: `https://YOUR-USERNAME.github.io/YOUR-REPO/admin.html`
- Add your Spotify account email under **User Management** in the app settings
- Copy your **Client ID** and paste it into `admin.html`
- Requires **Spotify Premium**

> **Note on Spotify API restrictions (February 2026):** Spotify restricted the `/search` and queue endpoints for new Development Mode apps. Auto-queuing via the API is not available for new apps. Track info and cover art are sourced from MusicBrainz instead — fully free with no restrictions. The Spotify integration provides a one-click button to open the track directly in Spotify for manual queuing.

### 3. GitHub Pages
- Push both files to a public GitHub repository
- Go to **Settings → Pages** and deploy from the `main` branch
- Your fan page will be live at `https://USERNAME.github.io/REPO-NAME`

---

## 🔄 Request Status Flow

```
Fan submits → Pending → Approved (appears on display + track info loaded) → Now Playing → Played
```

| Status | Description | Visible on display? |
|---|---|---|
| **Pending** | Awaiting DJ approval | ❌ No |
| **Approved** | DJ approved, track info and art loaded | ✅ Yes — in queue |
| **Now Playing** | Currently playing | ✅ Yes — highlighted |
| **Played** | Marked as done, greyed out | ❌ No |

---

## 🎵 Track Info & Cover Art

When a request is approved, the app automatically:
1. Searches **MusicBrainz** for the track using song name and artist
2. Fetches **album artwork** from Cover Art Archive
3. Displays cover art on the request card, display screen queue, and fullscreen view
4. Shows a **Spotify** button to open the track directly in Spotify

No API keys required for track search or artwork — both services are completely free and open.

---

## 📺 Display Screen

The Display tab shows a full-screen view for a TV or second monitor:
- Large **Now Playing** section with album artwork
- **Up Next** queue showing the next 6 approved requests with thumbnails
- Fullscreen mode opens in a separate window — drag to your second monitor

---

## 🖥️ Desktop App (optional)

An Electron-based installer kit is available for running Mii-Music as a standalone Windows desktop app.

**Requirements:** Node.js (from [nodejs.org](https://nodejs.org))

**Steps:**
1. Extract the installer kit folder
2. Double-click `BUILD.bat`
3. Wait 2-3 minutes
4. Find `Mii-Music Setup 1.0.0.exe` in the `dist/` folder

---

*Built with ❤️ by Mii-Media*
