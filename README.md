# CAREER-ARENA
# 🏟️ Career Arena

> **A real-time multiplayer career competition game** — Aptitude Quiz → Group Discussion → Interview, judged live by AI + Audience.

![Career Arena](https://img.shields.io/badge/Career%20Arena-Multiplayer-f5c518?style=for-the-badge)
![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js)
![Supabase](https://img.shields.io/badge/Supabase-Database-3ECF8E?style=for-the-badge&logo=supabase)
![Claude AI](https://img.shields.io/badge/Claude-AI%20Powered-cc785c?style=for-the-badge)
![Vercel](https://img.shields.io/badge/Vercel-Deployed-000000?style=for-the-badge&logo=vercel)

---

## 🎮 What is Career Arena?

Career Arena is a **real-time multiplayer web game** that simulates a real recruitment process as a competitive game. Players join a room, compete through three rounds, and get scored by both an **AI judge (Claude)** and a **live audience** — their fellow players.

Built for friend groups, college students, hackathons, or anyone who wants to practice interview skills in a fun, competitive format.

---

## 🔁 How It Works

### Round 1 — 🧠 Aptitude Quiz
- All players answer **10 questions simultaneously**
- Questions randomly picked from a **40+ question bank** across Logical Reasoning, Numerical, Verbal Ability, and General Knowledge
- **25 seconds** per question with a live countdown timer
- Instant feedback with explanations after each answer
- Score **5 or more** to advance to Round 2

### Round 2 — 🗣️ Group Discussion
- A **random GD topic** revealed to all players
- **3 minutes** of silent preparation time with private notes
- Each player submits a **60-second speech**
- After all speeches:
  - 🤖 **Claude AI** scores every player (1–10) with a verdict
  - 👥 **Audience** scores each player via peer ratings
  - Both scores shown side-by-side: `ADVANCE / BORDERLINE / ELIMINATED`
- Players vote for who advances to the Interview round

### Round 3 — 💼 Interview
- Each player answers **3 interview questions** (Behavioural, Situational, HR)
- After each answer:
  - 🤖 **AI Interviewer** gives score (1–10) + Strengths + Improvements
  - 👥 **Audience** rates independently
  - Live combined scoreboard updates in real-time
- Audience members react: Hire / Maybe / Pass

### 🏆 Final Results
- Combined score across all 3 rounds:
  - Quiz: **25%** · GD: **35%** · Interview: **40%**
- Podium with 🥇🥈🥉 and confetti for the winner

---

## ✨ Features

- 🔴 **Real-time multiplayer** — all players synced via Supabase
- 🤖 **AI-powered** — Claude AI scores GD, reviews interview answers
- 👥 **Dual scoring** — every round judged by both AI and audience
- 🃏 **40+ question bank** — different questions every game
- 🎯 **10 GD topics** — randomly selected each session
- 🔒 **Room system** — 5-letter room code, share with friends
- 📱 **Works on any device** — phone, tablet, laptop
- ⚡ **No login required** — just a name and room code

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML / CSS / JS |
| Backend | Node.js + Express |
| Database | Supabase (PostgreSQL) |
| Realtime | Supabase Realtime |
| AI | Anthropic Claude API |
| Hosting | Vercel |

---

## 🚀 Getting Started

### Prerequisites
- Node.js v18+
- Free [Supabase](https://supabase.com) account
- [Anthropic API key](https://console.anthropic.com)

### 1. Clone the repo
```bash
git clone https://github.com/yourusername/career-arena.git
cd career-arena
```

### 2. Install dependencies
```bash
npm install
```

### 3. Set up the database
- Go to Supabase → **SQL Editor** → **New Query**
- Paste contents of `supabase_setup.sql` → click **Run**

### 4. Configure environment variables
Create a `.env` file in the root:
```env
SUPABASE_URL=https://your-project-id.supabase.co
SUPABASE_ANON_KEY=your-anon-key-here
ANTHROPIC_API_KEY=sk-ant-api03-your-key-here
PORT=3000
```

### 5. Add the frontend
- Copy `index.html` into the `public/` folder
- Update the Supabase config at the top of the file:
```js
const SUPABASE_URL = 'https://your-project-id.supabase.co';
const SUPABASE_ANON = 'your-anon-key-here';
```

### 6. Run locally
```bash
npm start
```
Visit `http://localhost:3000` — game is live!

```bash
npm run dev   # auto-restart on file changes
```

---

## ☁️ Deploying to Vercel

### 1. Install Vercel CLI
```bash
npm install -g vercel
```

### 2. Add `vercel.json` to your project root
```json
{
  "version": 2,
  "builds": [
    { "src": "server.js", "use": "@vercel/node" }
  ],
  "routes": [
    { "src": "/(.*)", "dest": "server.js" }
  ]
}
```

### 3. Deploy
```bash
vercel
```

### 4. Add environment variables
Vercel Dashboard → Your Project → **Settings → Environment Variables**:

| Name | Value |
|------|-------|
| `SUPABASE_URL` | Your Supabase project URL |
| `SUPABASE_ANON_KEY` | Your Supabase anon key |
| `ANTHROPIC_API_KEY` | Your Anthropic API key |

### 5. Redeploy
Deployments tab → **Redeploy** — your game is live at `your-app.vercel.app` 🎉

---

## 📡 API Reference

### Room Endpoints — `/api/room`

| Method | Route | Description |
|--------|-------|-------------|
| `POST` | `/create` | Create a new room |
| `POST` | `/join` | Join an existing room |
| `GET` | `/state?roomCode=XX` | Get room + all players |
| `POST` | `/update_room` | Update room phase (host only) |
| `POST` | `/update_player` | Save player scores / speech |
| `POST` | `/vote` | Submit audience vote |
| `GET` | `/get_votes?roomCode=XX&round=gd` | Get all votes for a round |

### AI Endpoint — `POST /api/ai`

| Task | Payload | Description |
|------|---------|-------------|
| `gd_speech` | `{ name, topic }` | Generate AI player GD speech |
| `gd_score` | `{ topic, speeches[] }` | Score all GD players |
| `iv_review` | `{ question, category, answer }` | Review interview answer |

### Health Check
```
GET /health → { status: "ok", time: "..." }
```

---

## 📁 Project Structure

```
career-arena/
├── server.js                 ← Express entry point
├── routes/
│   ├── room.js               ← Room, player, vote management
│   └── ai.js                 ← Claude AI proxy
├── public/
│   └── index.html            ← Full game frontend
├── supabase_setup.sql        ← Run once to create DB tables
├── vercel.json               ← Vercel deployment config
├── package.json
├── .env                      ← Your secrets (never commit this!)
└── README.md
```

---

## 💰 Cost

| Service | Plan | Cost |
|---------|------|------|
| Vercel | Hobby (free) | $0/month |
| Supabase | Free tier | $0/month |
| Anthropic API | Pay-as-you-go | ~$0.003/game |

A full game with 4 players costs less than **half a cent** in API calls.

---

## 🤝 Contributing

PRs welcome! Ideas:
- More aptitude questions
- More GD topics
- Spectator mode
- Cross-session leaderboard
- Voice input for speeches
- Synchronized quiz timer across all players

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 🙏 Built With

[Express.js](https://expressjs.com) · [Supabase](https://supabase.com) · [Anthropic Claude](https://anthropic.com) · [Vercel](https://vercel.com)
