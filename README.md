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
- **Approval flow** — review every request before it goes public on the display screen
- **Spotify integration** — on approval, automatically searches Spotify for the track, shows cover art, and adds it to the Spotify playback queue
- **Filter requests** by type: All / Pending / Approved / Songs / Karaoke
- Mark requests as **Now Playing**, **Played**, or **Remove**
- **QR code generator** — customise with DJ name, event and venue, generate and share instantly
- **Auto-refresh** every 60 seconds as a backup to real-time sync
- **Display screen** — clean full-screen view showing Now Playing + queue with album artwork, designed to be shown on a TV or second monitor

---

## 🛠️ Built With

| Technology | Purpose |
|---|---|
| **HTML / CSS / JavaScript** | Frontend — no framework, runs in any browser |
| **Supabase** | Real-time database and live sync across devices |
| **Spotify Web API (PKCE)** | Track search, cover art, and auto-queue |
| **QRCode.js** | Client-side QR code generation |
| **GitHub Pages** | Free static hosting |
| **Electron** *(optional)* | Package as a installable desktop app |

---

## 📁 Files

| File | Description |
|---|---|
| `index.html` | Fan-facing request page — this is what fans see when they scan the QR code |
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
- Create an app at [developer.spotify.com](https://developer.spotify.com)
- Add `https://YOUR-GITHUB-USERNAME.github.io/YOUR-REPO/admin.html` as a Redirect URI
- Copy your **Client ID** and add it to `admin.html`
- Requires **Spotify Premium** for queue functionality

### 3. GitHub Pages
- Push both files to a public GitHub repository
- Go to **Settings → Pages** and deploy from the `main` branch
- Your fan page will be live at `https://USERNAME.github.io/REPO-NAME`

---

## 🔄 Request Status Flow

```
Fan submits → Pending → Approved (appears on display) → Now Playing → Played
```

- **Pending** — awaiting DJ approval, not visible on display
- **Approved** — DJ approved, joins the queue on the display screen, auto-added to Spotify
- **Now Playing** — currently playing, shown in the Now Playing section
- **Played** — marked as done, greyed out

---

## 📺 Display Screen

The Display tab shows a full-screen view intended for a TV or second monitor behind the DJ booth:
- Large **Now Playing** section with album artwork (via Spotify)
- **Up Next** queue showing the next 6 approved requests with thumbnails
- Fullscreen mode opens in a new window

---

*Built with ❤️ by Mii-Media*
