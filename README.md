# SONORA

**Sound, made visible.**

SONORA is a premium, immersive, audio-reactive 3D visualization studio built for the browser. Upload a track, and watch a distorted, glowing 3D form pulse, twist, and shimmer in real time — driven entirely by the actual frequency data of your audio, not pre-baked animation.

🔗 **Live demo:** [sonora-ha-lemon.vercel.app](https://sonora-ha-lemon.vercel.app)

---

## Features

- 🎧 **Real audio analysis** — Web Audio API `AnalyserNode` extracts live bass, mid, and treble energy from any uploaded track
- 🌐 **Reactive 3D scene** — a distorted icosahedron + glow sphere + particle field built with Three.js, deforming and rotating in response to the music
- 🎛️ **Three visual presets** — *Nebula*, *Pulse*, and *Vortex*, each visually distinct
- 🎚️ **Live controls** — bass sensitivity, distortion, rotation speed, particle density, and accent color, all adjustable while a track plays
- 🖱️ **Parallax** — mouse-move and scroll-driven camera movement
- 🎬 **GSAP scroll storytelling** — cinematic hero, scroll-triggered reveals, and a guided "Anatomy of Sound" section
- 👤 **Accounts & cloud saves** — Supabase-powered auth with Row Level Security; save/load visual presets tied to your account (falls back to local browser storage in guest mode)
- 📸 **Screenshot export** — capture the live canvas as a PNG
- 📱 **Responsive** — works across desktop and mobile, with safe-area-aware layout

## Tech stack

- **Three.js** (r128) — 3D rendering
- **Web Audio API** — real-time frequency/time-domain analysis
- **GSAP + ScrollTrigger** — animation and scroll-driven sequencing
- **Supabase** — authentication and Postgres-backed project storage (with Row Level Security)
- Plain HTML/CSS/JS — no build step required

## Getting started

This is a single self-contained `index.html` file — no `npm install`, no bundler.

### Run it locally
```bash
git clone https://github.com/hansolo6465-design/sonora.git
cd sonora
python -m http.server 8080
```
Then open `http://127.0.0.1:8080` in your browser.

### Enable accounts & cloud project saving (optional)
By default the app runs in **guest mode** (projects save to `localStorage` only). To enable real accounts:

1. Create a project at [supabase.com](https://supabase.com).
2. Run this in the Supabase SQL editor:
   ```sql
   create table projects (
     id uuid primary key default gen_random_uuid(),
     user_id uuid references auth.users not null,
     name text not null,
     preset text not null,
     settings jsonb not null,
     is_public boolean default false,
     created_at timestamptz default now(),
     updated_at timestamptz default now()
   );
   alter table projects enable row level security;
   create policy "select own" on projects for select using (auth.uid() = user_id);
   create policy "insert own" on projects for insert with check (auth.uid() = user_id);
   create policy "update own" on projects for update using (auth.uid() = user_id);
   create policy "delete own" on projects for delete using (auth.uid() = user_id);
   ```
3. In `index.html`, set your project's URL and publishable (anon) key:
   ```js
   const SUPABASE_URL = 'https://your-project.supabase.co';
   const SUPABASE_ANON_KEY = 'sb_publishable_xxxxxxxxxxxx';
   ```

### Deploy
This project deploys as-is to any static host:
- **Vercel** — import the repo, framework preset "Other," no build command needed
- **GitHub Pages** — enable Pages on the `main` branch, root folder

## Roadmap

- [ ] Video export via `MediaRecorder`
- [ ] Public project sharing
- [ ] Additional presets

## License

All rights reserved — original visual identity and code for the SONORA project.
