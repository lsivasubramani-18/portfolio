# Sivasubramani L — Portfolio

A colorful, editorial personal portfolio for an Electronics & Communication Engineering student, with a private admin panel for managing every piece of content.

- **Stack:** React, Vite, React Router, Framer Motion, Lucide React, plain CSS
- **Storage:** everything lives in the visitor's browser — `localStorage` for content and settings, `IndexedDB` for uploaded images, videos and PDFs
- **No backend, no API keys, no `.env` file, no external accounts**

## Run locally

```bash
npm install
npm run dev
```

Open the URL Vite prints (usually http://localhost:5173).

```bash
npm run build     # production build into dist/
npm run preview   # serve the production build locally
```

## Admin panel

The admin lives at `/admin` (it redirects to `/admin/login` when signed out). It is intentionally **not linked anywhere** on the public site.

Local development credentials:

```text
Email:    admin@portfolio.local
Password: Portfolio@2026
```

From the admin you can edit the profile and photo, and add / edit / delete / hide / show / reorder skills, interests, projects, symposiums, presentations and workshops, fill in contact details, change the accent color, toggle reduced motion, and reset all data (with confirmation).

### Important: how the data and login really work

- The login is a **browser-level access gate only**. It is not server-side security. Anyone able to inspect the site's JavaScript could work out how it functions, and the checked-in credentials above are for local convenience.
- Because there is no backend, **content is saved in the browser you edit from**. Edits made on your laptop are not visible to visitors on other devices, and deploying to Vercel does not carry your browser data with it. On a deployed site, the public sees the built-in defaults (name, role, about text, 8 skills, 3 interests) unless a backend is added.
- To publish real content to everyone, the next step is a real backend and real authentication. The code is organised for that:
  - `src/data/portfolioStore.js` — content API (`usePortfolio`, `updatePortfolio`, `resetPortfolio`)
  - `src/data/mediaStore.js` — media API (`saveMedia`, `getMedia`, `deleteMedia`, …)
  - `src/data/authStore.js` — `login`, `logout`, `useAuth`

  Replace the bodies of those three files and the UI keeps working.

## Content rules

Only the information provided by the owner is seeded. Projects, symposiums, presentations, workshops, contact details and the profile photo all start empty and show designed empty states. Nothing is invented.

## Deploy to Vercel (via GitHub)

1. Create a new repository on GitHub.
2. In the project folder:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
3. On [vercel.com](https://vercel.com), choose **Add New → Project** and import the repository.
4. Vercel detects **Vite** automatically (build command `npm run build`, output directory `dist`). Click **Deploy**.

`vercel.json` already rewrites all routes to `index.html`, so `/admin` and `/admin/login` work after deployment. No environment variables are needed.

## Project structure

```text
src/
├── main.jsx, App.jsx        routing, theme + motion settings
├── components/              public site sections, modal, toast, media
├── admin/                   login, layout, dashboard and editors
├── data/                    defaults + storage abstraction (store / media / auth)
├── utils/helpers.js
└── styles/global.css
```

## Media limits

Images: JPG, JPEG, PNG, WEBP (8 MB). Videos: MP4, WebM (60 MB) or an external link (YouTube, Vimeo, direct file). PDFs: 20 MB.
