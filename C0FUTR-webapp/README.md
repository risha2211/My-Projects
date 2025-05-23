# 🌱 GREEN (C0FUTR): Campus Web App for Climate Action and Sustainability

## 🧭 Overview
**GREEN** is a modern web-based platform that gamifies sustainability and climate action on college campuses.  
It helps users track eco-behaviors, take part in challenges, and build community — all while reducing emissions and building campus resilience.

---

## 🌍 Key Benefits

- 🌿 Cuts emissions and waste  
- 👥 Boosts community engagement  
- 🛡️ Enhances disaster/climate resilience  
- 💸 Saves operational costs  
- 🧑‍🎓 Builds sustainability leadership  
- 📈 Improves institutional image and rankings  

---

## 🧠 Core Features

### 1. Personalized Sustainability Tracker
- Set and monitor eco-goals (waste, energy, water, carbon)
- Compare progress with peers, hostels, or clubs
- View weekly/monthly/yearly sustainability reports

### 2. Gamified Challenges
- Daily/weekly/monthly challenges with XP rewards
- Live leaderboards with rival tracking
- Rank-based progression (e.g., Rookie → Rainmaker)
- Digital badges, certificates, and store coupons for achievers

### 3. Peer Engagement & Social Sharing
- Upload photos/videos of green actions
- Peer validation & community support
- Event creation, collaboration boards, and chat rooms

### 4. Eco-Learning Hub
- Bite-sized infographics, tutorials, and videos
- Guided tours of campus sustainability facilities
- Links to masterclasses and certifications

---

## 🛠️ Technical Architecture

### 🔹 Frontend
- React.js + Tailwind CSS for responsive, clean UI
- Framer Motion for animations and smooth interactions
- Progressive Web App (PWA) support for mobile and desktop

### 🔹 Backend
- Node.js + Express.js for RESTful APIs
- MongoDB or Firebase Firestore for real-time NoSQL storage
- Redis for fast caching (leaderboards, challenge cooldowns)

### 🔹 Authentication
- Firebase Auth (Email, Google, Institute SSO)
- Role-based access (User, Moderator, Admin)

### 🔹 Gamification Engine
- Custom logic layer for:
  - XP tracking
  - Rank/badge calculation
  - Task weightage & honesty multiplier
  - Cooldown timers and streaks

### 🔹 Image & Video Verification
- Firebase Storage / AWS S3 for uploads
- Metadata extraction (timestamp, GPS, hash)
- Optional ML validator to detect fakes/duplicates
- Manual peer + admin reviews for key tasks

### 🔹 Notifications & Alerts
- Push notifications via Firebase Cloud Messaging
- Streak reminders, rank-up popups, and task cooldowns
- Scheduled reminders via cron jobs

### 🔹 Analytics & Reporting
- Dashboards with Recharts / Chart.js
- Admin panels with filters & CSV export
- Campus-wide sustainability stats

### 🔹 Scalability & Hosting
- Hosted on Vercel / Firebase Hosting / Render
- CDN delivery for media
- Multi-campus support via subdomains or tenant DBs

---

## 🔒 Verification & Security

- Peer validation + admin approval for high-impact reports
- Photo/video with timestamp & GPS validation
- ML and manual audits for fairness
- Anti-spam + media reuse checks
- Encrypted user data + 2FA for admin access

## 🏅 Rank & Reward System

| Tier      | Description                          |
|-----------|--------------------------------------|
| 🟫 Bronze  | Basic recycling (paper, plastic)     |
| 🥈 Silver  | E-waste, battery recycling           |
| 🥇 Gold    | Donating items, upcycling            |
| 💎 Platinum | Leading campus-wide impact drives   |

## 🏆 Achievement Titles & Badges

### 🎖️ Progression Titles
Users level up based on XP, integrity score, and peer-reviewed task quality:

`Rookie` → `Rebel` → `Ranger` → `Rainmaker`

### 🧩 Bonus Badges
Awarded for specific milestones and behaviors:

- **Streak Keeper** – For maintaining consistent daily activity
- **Eco Leader** – For organizing community events or drives
- **Verified Achiever** – For highly rated, peer-verified submissions
---
## 🔁 Smart Waste & Reuse Programs

- 📊 **Integration with Campus Waste Logs**  
  Syncs with existing waste collection databases to track volume and type of waste.

- 🗑️ **Smart Bin Support (Optional)**  
  IoT-enabled bins using Arduino or Raspberry Pi to detect waste type, fill levels, and usage patterns.

- 🔧 **DIY Repair Workshops & Digital Swap Boards**  
  Promote repair culture through campus workshops.  
  Virtual boards to list/exchange usable items (e.g., books, appliances, clothes).

- 🎁 **Hostel Donation Drives**  
  Organized at semester-end for clothes, books, and electronics reuse among students.

## 📊 Realistic Campus Targets

| **Metric**              | **Target** | **Benchmark Institutes**                  |
|-------------------------|------------|-------------------------------------------|
| Waste Reduction         | 30%        | IIT Bombay: 30%, IIM A: 25%               |
| Recycling Rate          | 60%        | IIT Madras: 60%, DU: 55%                  |
| Student Engagement      | 75%        | Internal via events & posts               |
| Energy Use Reduction    | 15%        | IISc: 10%, DU: 15%                         |

## 💸 Institutional Savings & Impact

| **Institute**       | **Savings Achieved** | **Methods Used**                    |
|---------------------|----------------------|-------------------------------------|
| IIT Bombay          | ₹1.5 Cr/year         | Energy audits, renewables           |
| Delhi University    | ₹2.5 Cr/year         | Waste reforms, awareness            |
| IISc Bangalore      | ₹1.2 Cr/year         | Smart metering, policies            |

## 🧭 Roadmap & Scalability

### Pilot Phase (1 Campus)
- Core features (tracking, challenges, leaderboards)
- Moderate reward system and peer verification

### Phase 2
- ML-based photo verification
- Smart bin integration
- Offline QR-scan log-in for events & workshops

### Phase 3
- Regional leaderboard across campuses
- Hackathons and community rewards
- Integration with sustainability credits or attendance

---

## 🚀 Final Vision

**GREEN (C0FUTR)** aims to become the digital foundation of campus climate action.  
By combining technology, gamification, and community, it transforms everyday habits into powerful environmental movements.

[Index](./C0FUTR_LINKS/index.html)  
[Wallet](./C0FUTR_LINKS/wallet.html)
[Games](./C0FUTR_LINKS/gamification.html)  
[Portfolio](./C0FUTR_LINKS/portfolio.html)
[Eco-Tasks](./C0FUTR_LINKS/eco-task.html)  
[Rewards](./C0FUTR_LINKS/rewards.html)
[Challenges](./C0FUTR_LINKS/community-challenges.html)  









