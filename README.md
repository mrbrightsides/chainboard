# ChainBoard 📝

Mini ClickUp + LM-powered Meeting Summary, Realtime with PlaytimeDB

ChainBoard adalah mini project management app yang memungkinkan kamu mengelola task, project, dan meeting secara realtime, plus ada fitur AI untuk bikin summary meeting otomatis. Dibangun dengan vibes RANTAI / chain-nya tetep kerasa, tapi ringan & gampang dipakai.

---

## ⚡ Features

- Kanban-style Task Board: drag & drop task antar status (To Do, Doing, Done)

- Project Management: tambah, edit, hapus project; expand project → show tasks

- Meeting Module: catat meeting, invite participants, generate AI summary + action items

- Realtime Collaboration: update task & project langsung realtime antar device via PlaytimeDB

- Notifications (Mini): task due soon, new comment / update

---

## 🛠 Tech Stack

- Frontend: Next.js + TailwindCSS + Zustand

- Database / Realtime: PlaytimeDB

- Language Model (LM): OpenAI API atau LM ringan lokal

- Auth (optional): email login / wallet Web3 login (untuk fun badges) -- enroute

- Deployment: Vercel / Netlify

---

## 📦 Data Structure (PlaytimeDB)
```bash
Project:
  - id, name, description, created_at
  - tasks -> Task[]

Task:
  - id, title, description, status, assigned_to, due_date, created_at
  - comments -> optional

Meeting:
  - id, title, participants, notes, summary, date_time

User:
  - id, name, email, avatar_url
```

---

## 🚀 Quick Start

Clone repo

```bash
git clone https://github.com/username/chainboard.git
```

Install dependencies

```bash
cd chainboard
npm install
```

Setup PlaytimeDB

Buat project baru di PlaytimeDB

Ambil API key & masukkan ke .env.local

Run development server

```bash
npm run dev
```

Open app: http://localhost:3000

---

 ## 🧠 How LM Summary Works

User input meeting notes → call LM API → generate short summary + action items

Summary tersimpan di PlaytimeDB, bisa langsung di-assign ke task/project

---

## 💡 Roadmap

 Filter & search tasks / projects

 Due date reminders & notifications

 Assign badges / points via Web3 wallet

 Dark mode & minor animations

 Mobile responsive polish

---

## 📜 License

MIT License — bebas dipakai, dimodifikasi, & dibagi wkwkw 😎
