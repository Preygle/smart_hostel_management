# SmartHostel AI 🏠

> **Intelligent Hostel Operations & Management Platform**  
> Solve-A-Thon 2026 · VIT Chennai · PS-002 · Team of 5

[![Stack](https://img.shields.io/badge/Stack-React%20%7C%20Node.js%20%7C%20MongoDB-blue)]()
[![AI](https://img.shields.io/badge/AI-Google%20Gemini%20API-purple)]()
[![Real-time](https://img.shields.io/badge/Real--Time-Socket.io-black)]()
[![Database](https://img.shields.io/badge/Database-MongoDB%20Atlas-green)]()
[![Frontend](https://img.shields.io/badge/Frontend-Vite%20%7C%20React%2019-cyan)]()


---

## ✅ Problem Statement Requirement Coverage

| # | Requirement | Module | Status |
|---|---|---|---|
| 1 | Student records & room allocation | Hostel Info (Block → Floor → Room Explorer) | ✅ |
| 2 | Maintenance requests & complaints | Smart Complaint System (Dual-Severity NLP AI) | ✅ |
| 3 | Late entry / activity logs | Gatepass + Attendance + Students Desk | ✅ |
| 4 | Admin dashboard | Real-time Analytics Dashboard | ✅ |
| 5 | Student information access | Full Student Self-Service Portal | ✅ |
| + | Data privacy & RBAC | JWT Auth + Role-based middleware | ✅ |
| + | AI Intelligence Layer | OpenRouter LLM + Face Liveness Detection | ✅ |

---

## 🚀 Complete Feature List

---

### 🤖 AI CHATBOT — SmartHostel AI Assistant

The flagship feature. Replaces all hardcoded frontend rule-based logic.

| Sub-Feature | Details |
|---|---|
| **LLM Engine** | OpenRouter `qwen/qwen3.6-plus:free` model via official `openai` SDK |
| **Natural Language Understanding** | Parses typos, casual speech, "story-based" excuse writing, multilingual intent |
| **Structured JSON Responses** | LLM outputs `{ type, text, preview, actions }` — no markdown wrapping |
| **Dynamic Preview Cards** | Chat bubble renders interactive Submit/Cancel cards from `type: "preview"` |
| **Multi-turn Memory** | Full conversation history is sent on every request for contextual continuity |
| **Context Awareness** | Auto-fetches user room, laundry day, mess crowd, complaints & gatepass history via `GET /api/v1/chatbot/context` |
| **Outing Validation** | LLM enforces 8AM–6PM only, max 2h weekdays / 6h weekends — rejecting violations naturally |
| **Leave Validation** | LLM enforces minimum 24-hour / 1 full-day leave duration |
| **Error Resilience** | Strips markdown code fences from LLM output before JSON parsing; falls back gracefully |
| **Chatbot Float Pattern** | ChatBot bubble mounts in `AppShell.jsx` bottom-right; state persists across route changes |

---

### 🚪 GATEPASS & LEAVE SYSTEM

| Sub-Feature | Details |
|---|---|
| **Apply Gatepass / Outing** | Form-based apply with time pickers, destination, reason fields |
| **Apply Leave** | Separate Leave form, requires guardian name + phone + relation |
| **Type Enforcement** | Types: `Outing`, `Leave`, `Hospital` — each with unique validation |
| **Outing Rules** | 8AM–6PM only; same-day; max 2h weekdays / 6h weekends |
| **Leave Rules** | Minimum 1 full day (24h); no maximum duration |
| **QR Code Generation** | Warden-approved gatepasses get a unique `qr_token` rendered as scannable QR |
| **Guard Entry/Exit Scan** | Guards scan QR at hostel gate to mark `actual_exit` and `actual_return` |
| **Status Lifecycle** | Pending → Approved / Rejected → Active → Returned / Expired / Recalled |
| **Overdue Detection** | `is_overdue` flag auto-set when `actual_return > expected_return` |
| **Late Return Window** | Students get 1-hour window after overdue return to submit excuse text |
| **My Gatepass View** | Student portal timeline with status badges, dates, destination, QR preview |
| **Admin Gatepass Management** | Warden approve/reject with optional note; full log view |

---

### 🏃 STUDENTS DESK (ADMIN — LATE RETURN MANAGEMENT)

| Sub-Feature | Details |
|---|---|
| **Late Arrivals Feed** | Live list of all students with `is_overdue` gatepasses |
| **Student Excuse Display** | Shows student-submitted excuse text with timestamp |
| **Warden Decision** | Warden selects: Ask To Meet / Clear Student / Portal Call Follow-up |
| **Warden Message** | Typed message saved and displayed to guard at re-entry scan |
| **🔊 Audio Warden Note** | "Listen" button plays TTS of the warden's note (base64 audio stream) |
| **Portal Voice Call** | Real-time in-browser WebRTC/WebSocket call between warden and student |
| **Call Transcript Log** | After call, warden pastes transcript or uses Sarvam AI transcription |
| **Call Not Picked Flag** | One-click mark if student doesn't answer |
| **Flagged Students Panel** | Searchable list of all students with active flags, sorted by weighted severity score |
| **Severity Scoring** | `suspicious_flag_count ×4` + `outing_flag_count ×3` + `community_strikes ×2` + `dhobi_offence ×1` |

---

### 📋 COMPLAINTS SYSTEM

| Sub-Feature | Details |
|---|---|
| **Raise Complaint** | Category, description, severity — submitted by student |
| **AI Dual-Severity Scoring** | NLP urgency detection on student text + system-level rules (e.g. Electrical→High, Plumbing leak→High); takes `MAX()` |
| **Categories** | Electrical, Plumbing, Civil, Housekeeping, Pest Control, Internet, Other |
| **SLA Timers** | Each complaint tracked against expected resolution time |
| **Systemic Flag Detection** | Admin flags complaints affecting multiple rooms as `is_systemic` |
| **Complaint Heatmap** | Admin analytics showing complaint density by floor |
| **My Complaints View** | Student sees personal complaint history with status badges |
| **Admin Complaint Dashboard** | Full overview with priority queue, status filters, and SLA breach indicators |

---

### 🧾 ATTENDANCE SYSTEM

| Sub-Feature | Details |
|---|---|
| **Night Attendance Window** | Warden opens/closes attendance window per block — students can only check-in when open |
| **Face Liveness Detection** | Student clicks "Mark Attendance" → opens `FaceCheckModal` → webcam liveness check |
| **Face Check Modal** | `FaceCheckModal.jsx` handles webcam access, liveness blink/nod detection, confidence score |
| **30-second Auto-Poll** | Attendance page polls window status every 30s; shows 🟢 OPEN / 🔴 CLOSED live |
| **Attendance History** | Last 30 days records with status (Present / Absent / On Leave / On Outing) |
| **Attendance Ring Chart** | SVG ring showing % attendance colored green/yellow/red |
| **Status: On Leave / On Outing** | Gatepass system auto-marks attendance when student is out |
| **Admin Attendance View** | Floor-level attendance grid; anomaly detection for 3+ consecutive absences |

---

### 🍽️ MESS SYSTEM

| Sub-Feature | Details |
|---|---|
| **Weekly Menu** | Full 7-day menu across Veg, Non-Veg, and Special caterers |
| **4 Meals Per Day** | Breakfast, Lunch, Snacks, Dinner — each with items, caterer, and nutrition |
| **Nutrition Pills** | Per-meal Protein / Carbs / Fat / Fiber / Kcal displayed as compact pills |
| **Live Crowd Prediction** | Time-of-day heuristic algorithm (peak / off-peak / very high) with fill % |
| **Crowd Alert Banner** | Warning banner shown when mess fill % is above 70% |
| **Meal Attendance Marking** | Student marks `Ate` / `Skipped` per meal for crowd prediction use |
| **Anonymous Feedback System** | Student rates Taste / Quality / Quantity / Hygiene / Variety (1–5) per meal — identity hidden from staff |
| **Day Selector** | Tab bar to view any day's menu with Mon–Sun navigation |
| **Admin Mess Management** | Warden/Admin edits weekly menu, assigns caterers, manages night mess items |

---

### 🌙 NIGHT MESS

| Sub-Feature | Details |
|---|---|
| **Browse Night Menu** | Available items with name, description, category, prep time, stock count, price |
| **Cart & Quantity** | +/– quantity per item; total automatically computed |
| **Pre-pay & Order** | Advance payment model — order is placed and paid before preparation |
| **Order History** | Student sees all past night orders with status: Pending / Ready / OutOfStock / NotCollected |
| **Refund Policy** | Out-of-stock → full refund; Not collected → 25% fine, rest refunded |
| **Fine & Refund Display** | Shows refund_amount, fine_amount, note in order history |

---

### 👕 LAUNDRY (CHOTA DHOBI)

| Sub-Feature | Details |
|---|---|
| **Room-Based Schedule** | Laundry day deterministically assigned from room number (`room_no % 7`) |
| **Is Laundry Day Detection** | Page shows context-aware state — "Not Your Day" or drop-off QR |
| **Bag Drop-Off QR** | On laundry day, student shows QR to Dhobi staff to submit bag |
| **Processing Status** | After drop-off, status changes to "Processing" with spinner |
| **Ready for Pickup Banner** | When laundry is done, a green "Ready for Pickup!" banner replaces the QR |
| **Socket.io Real-time** | `laundry:accepted`, `laundry:ready`, `laundry:out_of_schedule` events pushed live |
| **Weekly Schedule Table** | Full day-by-day schedule table showing room ranges and room counts |
| **Laundry Offence Tracking** | Dropping bags outside schedule increments `dhobi_offence` flag on student profile |

---

### 🏠 HOSTEL INFO (ROOM EXPLORER — ADMIN)

| Sub-Feature | Details |
|---|---|
| **Multi-Block Selector** | A Block (live), B/C/D1/D2/E blocks (locked/coming soon) |
| **Floor Stack Panel** | Visual vertical stack of all 15 floors — hover to highlight, click to select |
| **Floor Canvas** | 2D floor plan of all rooms in the selected floor, color-coded by occupancy |
| **Complaint Heatmap Overlay** | Floor canvas overlaid with complaint density data per floor |
| **Room Detail Modal** | Click any room → modal shows beds (AC/NAC), occupants, room type, bed assignments |
| **Bed Assignment** | Admin can assign a student to a vacant bed from the modal |
| **Block Summary Stats** | Total floors / rooms / avg occupancy shown in the floor selector pane |

---

### 💬 COMMUNITY FORUM (HOSTEL COMMUNITY)

| Sub-Feature | Details |
|---|---|
| **Pseudonymous Posts** | Every student gets a random pseudonym + avatar color — identity hidden from peers |
| **Categories** | General, Lost & Found, Book Exchange, Events, Questions, Memes, Rant, Hostel Feedback |
| **Sort Modes** | Hot (engagement-weighted), New, Top (all-time votes) |
| **Voting** | Upvote / Downvote on posts and replies; live vote score update |
| **Threading** | Nested replies under each post with their own vote counts |
| **Tag System** | Comma-separated optional tags on posts; trending tag cloud in sidebar |
| **Full-text Search** | Keyword search across post titles and content |
| **Trending Sidebar** | Hot posts today, trending tag cloud, hot categories |
| **Hostel Feedback Visibility** | Posts in "Hostel Feedback" category are flagged as visible to wardens |
| **AI Moderation** | Post content runs through toxicity scoring; flagged if over threshold |
| **Strike System** | 3 strikes → automatic community ban; warden can lift ban |
| **Banned State** | Banned students see ban banner; create post and reply disabled |
| **Admin Community Intelligence** | Mood Index (0–100), total posts, flagged count, banned user count |
| **Admin Flagged Posts** | Full list with toxicity %, reveal real identity (name + room + block) |
| **Ban Management Tab** | View all banned users; lift ban with one click |
| **Admin Trending Tab** | Tag cloud + hot posts + category activity analytics |
| **7-Day Activity Trend Chart** | Bar chart of daily post volume, colored by toxicity level |

---

### 🏥 HEALTH SOS

| Sub-Feature | Details |
|---|---|
| **Emergency Trigger** | Student one-tap SOS with severity level selection |
| **Simultaneous Alerts** | Alert dispatched to wardens, guards, and floor admins instantly via Socket.io |
| **Health Events Admin View** | Admin sees all SOS events with student info, severity, timestamp, resolution status |

---

### 📢 ANNOUNCEMENTS

| Sub-Feature | Details |
|---|---|
| **Admin Broadcast** | Admin creates announcements pushed to all connected students via Socket.io |
| **Student Announcement View** | Students see active announcements in a dedicated panel |

---

### 👔 STAFF DIRECTORY

| Sub-Feature | Details |
|---|---|
| **Staff Contact Cards** | Name, role, shift, contact number displayed for all hostel staff |
| **Roles Covered** | Warden, Guard, Housekeeping, Mess Staff, Admin |

---

### 👤 STUDENT PROFILE

| Sub-Feature | Details |
|---|---|
| **Profile View** | Student sees name, register number, room, floor, block, bed type, mess assignment |
| **Virtual Fields** | `floor_no`, `bed_id`, `mess_information` derived from schema virtuals |

---

### 🧑‍💼 GUEST REQUEST

| Sub-Feature | Details |
|---|---|
| **Guest Application** | Student requests an external visitor pass |
| **Warden Approval** | Warden reviews and approves/rejects guest request |
| **QR Guest Pass** | Approved guest gets a QR pass for guard scan at entry |

---

### 👤 PORTAL VOICE CALLS (WARDEN ↔ STUDENT)

| Sub-Feature | Details |
|---|---|
| **WebSocket Call** | In-browser voice call initiated from Students Desk by warden |
| **Student Call Widget** | Student receives ringing notification and can accept/reject |
| **Call Store** | Global Zustand store (`callStore.js`) manages call state across routes |
| **Language Hint** | Call language (Tamil / Hindi / Hinglish / English) stored per call |

---

### 📊 ADMIN ANALYTICS DASHBOARD

| Sub-Feature | Details |
|---|---|
| **Real-time Health Score Ring** | SVG ring chart showing overall hostel health % |
| **Live Charts** | Recharts-powered bar/line charts for attendance trends, complaint types |
| **Socket.io Live Alerts** | Push alerts as events happen (SOS, late return, new complaint) |
| **Complaint Heatmap** | Floor-level density chart of complaints |
| **Room Occupancy Stats** | Live fill rates from room collection |
| **Announcement Panel** | All active announcements visible from dashboard |

---

### 🔐 AUTHENTICATION & RBAC

| Sub-Feature | Details |
|---|---|
| **JWT Authentication** | 7-day token signed with `JWT_SECRET`; stored in localStorage |
| **5 Roles** | `student`, `warden`, `admin`, `security` (guard) |
| **Role-based Routing** | `App.jsx` redirects based on `user.role` post-login |
| **Middleware Guard** | `authenticate` middleware validates JWT; `authorize(roles)` checks role |
| **Route Protection** | Protected routes check token on every API call |

| Module | Student | Guard | Warden | Admin |
|---|---|---|---|---|
| Gatepass | Apply + view own | Scan QR | Approve / Reject | Full logs |
| Complaints | Raise + view own | — | View + update | Full analytics |
| Attendance | View own + face check-in | — | Open/close window | Full CRUD + anomaly |
| Community | Post + vote | — | View feedback | Moderate + ban |
| Room Explorer | — | — | Floor view | Block → Room full access |
| Students Desk | — | — | Full | Full |
| Night Mess | Order | — | Manage items | Full |
| Laundry | View + QR | Scan | — | Schedule admin |

---


### 🗄️ DATABASE SCHEMAS (MongoDB / Mongoose)

| Model | Key Fields |
|---|---|
| **User** | `name`, `register_number`, `password` (bcrypt), `role`, `room_no`, `floor_no`, `block_name`, `bed_type`, `bed_id`, `mess_information`, `is_flagged`, `outing_flag_count`, `suspicious_flag_count`, `community_strikes`, `dhobi_offence` |
| **Gatepass** | `student_id`, `type (Outing/Leave/Hospital)`, `destination`, `reason`, `expected_exit`, `expected_return`, `guardian_name/phone/relation`, `status`, `qr_token`, `actual_exit`, `actual_return`, `is_overdue`, `late_return_count`, `late_return` (sub-doc: excuse_text, warden_decision, call_status, call_transcript) |
| **Complaint** | `raised_by`, `title`, `category`, `severity`, `status`, `sla_breached`, `is_systemic`, `raised_at` |
| **Attendance** | `student_id`, `date`, `status`, `method (face/manual/wifi)` |
| **Mess** | `day`, `meal_type`, `caterer`, `items` (with nutrition), `menu_name` |
| **LaundrySession** | `student_id`, `status`, `qr_token`, `assigned_day` |
| **CommunityPost** | `author_id`, `pseudonym`, `avatar_color`, `title`, `content`, `category`, `tags`, `upvote_count`, `downvote_count`, `vote_score`, `toxicity_score`, `flagged`, `flag_reason`, `replies` (sub-docs) |
| **Announcement** | `title`, `content`, `created_by`, `active` |
| **HealthEvent** | `triggered_by`, `severity`, `location`, `resolved_at` |
| **Guest** | `student_id`, `guest_name`, `visit_date`, `status`, `qr_token` |
| **ChatSession** | `student_id`, `history` (messages array) |
| **Room** | `room_no`, `floor_no`, `block_name`, `beds` (array with `bed_id`, `student_id`, `is_occupied`, `type`) |
| **Block** | `name`, `total_floors`, `total_rooms` |
| **Staff** | `name`, `role`, `shift`, `phone`, `block_name` |

---

## 🏗️ Project Structure

```
smart_hostel_management/
├── client/
│   └── src/
│       ├── pages/
│       │   ├── student/        ← Dashboard, Mess, NightMess, Laundry,
│       │   │                      Attendance, MyGatepass, ApplyGatepass,
│       │   │                      MyComplaints, RaiseComplaint, Community,
│       │   │                      GuestRequest, NightMess, Profile
│       │   └── admin/          ← Dashboard, RoomAllocation, HostelInfo,
│       │                          AttendanceView, ComplaintDashboard,
│       │                          MessManagement, GatepassManagement,
│       │                          Announcements, StudentsDesk,
│       │                          CommunitySentiment, HealthEvents,
│       │                          StaffDirectory
│       ├── components/
│       │   ├── ChatBot/        ← ChatBot.jsx + handlers
│       │   ├── FaceCheckModal.jsx (liveness detection)
│       │   ├── calls/          ← Portal voice call components
│       │   └── hostel/         ← FloorStack, FloorCanvas, RoomDetailModal
│       ├── store/              ← Zustand: authStore, callStore
│       ├── lib/api.js          ← Axios instance + Vite proxy
│       └── hooks/useSocket.js  ← Socket.io connection hook
│
├── server/
│   └── src/
│       ├── routes/             ← auth, users, rooms, gatepass, complaints,
│       │                          attendance, mess, laundry, community,
│       │                          chatbot, announcements, health, guests,
│       │                          analytics, staff
│       ├── models/             ← All 14 Mongoose models
│       └── middleware/         ← auth.js (JWT), errorHandler.js, rbac.js
│
├── generate_hostel_db.py       ← Python MongoDB seeder
└── AI.md                       ← LLM context document
```

---

## 🚀 Quick Start

### 1. Clone & Install

```bash
git clone https://github.com/Preygle/smart_hostel_management.git
cd smart_hostel_management
cd server && npm install
cd ../client && npm install
```

### 2. Environment Setup (`server/.env`)

```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb+srv://<user>:<pass>@hostel-cluster.mongodb.net/?appName=hostel-cluster
JWT_SECRET=smarthostel_super_secret_2026
JWT_EXPIRES_IN=7d
CLIENT_URL=http://localhost:5173
openrouter-qwen=sk-or-v1-your-openrouter-key-here
gemini=your-gemini-key-here
```


### 3. Run

```bash
# Terminal 1 — Backend
cd server && npm run dev   # :5000

# Terminal 2 — Frontend  
cd client && npm run dev   # :5173
```

---

## 🔐 Demo Credentials

| Role | Register Number | Password |
|---|---|---|
| **Student** | `23BEC1106` | `S7wJ0UlaKN` |
| **Warden** | `114812` | `$2b$12$QRmZvMAjiePVrB1tv5Sf8Og//3JluqyZhFklmKhNHfBpPH1PJ4j4K` |
| **Dhobi** | `316600` | `$2b$12$LRjqA.G9YWWynWqFVFjTHuivwCj6ahXDi.DVX6EYd1.f9qeX8aiOm` |
| **Guard** | `114900` | `$2b$12$examplehashedpasswordstring` |

---


*SmartHostel AI · PS-002 · Solve-A-Thon 2026 · VIT Chennai*
