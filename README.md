# 🛂 NPDPS — National Passport Digital Processing System

> A production-grade, full-stack government passport portal simulation built with vanilla HTML, CSS, and JavaScript. No frameworks, no build tools, no backend server required — just open `index.html` and it runs.

---

## 🚀 Live Demo

Open `index.html` in any modern browser. No installation needed.

**Demo Credentials:**

| Role | Email | Password |
|------|-------|----------|
| Citizen / User | `rajesh@example.com` | `user123` |
| Admin Officer | `admin@npdps.gov.in` | `admin123` |

---

## 📸 Screenshots Overview

| Page | Description |
|------|-------------|
| Landing Page | Government banner, services, notices, FAQ |
| Login / Register | OTP verification, JWT-simulated sessions |
| Dashboard | Stats, active application tracker, notifications |
| Apply (5 Steps) | Multi-step form with auto-save draft |
| Track Status | Real-time timeline tracker |
| Appointments | Calendar slot booking system |
| Admin Panel | Approve/Reject/Dispatch + analytics |

---

## 🏗️ Architecture

This is a **single-file full-stack application** — the entire frontend and simulated backend live in one `index.html` file.

```
NPDPS/
│
├── index.html          ← Complete application (frontend + simulated backend)
└── README.md           ← This file
```

### How the "Backend" Works

There is no server. All data is persisted using the browser's `localStorage` as a simulated database. The architecture mirrors a real backend:

```
localStorage  ←→  DB Layer (get/set/init)  ←→  Page Functions  ←→  DOM
```

Data collections stored in localStorage:

| Collection | Description |
|------------|-------------|
| `npdps_users` | Registered citizen accounts |
| `npdps_applications` | All passport applications |
| `npdps_appointments` | Booked PSK appointments |
| `npdps_notifications` | Per-user notification feed |
| `npdps_slots` | Booked time slots (prevents double-booking) |
| `npdps_session` | Current logged-in user session |
| `npdps_draft` | Auto-saved application draft |

---

## ✨ Feature Breakdown

### 🔐 Authentication System
- Email + password login with credential validation
- New user registration with **6-digit OTP email verification** simulation
- Forgot password flow
- Persistent sessions (survives page refresh via localStorage)
- Role-based access control: `user` and `admin` roles
- Password show/hide toggle

### 📋 Multi-Step Application Form (5 Steps)
1. **Personal Details** — Name, DOB, gender, parents, PAN, mobile, Aadhaar simulation
2. **Address** — Present address, state dropdown (all 28 states + UTs), PSK center selection
3. **Police Verification** — Police station, jurisdiction, duration, declaration checkbox
4. **Emergency Contact** — Relation, phone, address, optional reference
5. **Documents & Review** — Upload simulation with preview, full summary, fee display, final declaration

**Advanced UX features:**
- Auto-save draft on every field change
- Field-level tooltips and helper text
- Conditional fields (e.g., old passport shown for renewal)
- Processing type toggle (Normal ₹1,500 / Tatkal ₹3,500)
- Fraud score auto-generated on submission

### 📊 Smart Dashboard
- Profile completion ring (SVG animated circle)
- 4 stat cards (total, in-progress, approved, notifications)
- Active application tracker with timeline
- Upcoming appointment display
- Notification panel (last 3 unread)

### 🕐 Real-Time Status Tracking
- Search by Application ID (public, no login needed)
- Full timeline: Submitted → Scrutiny → Police Verification → Approved → Dispatched
- Color-coded dots: green (done), pulsing gold (active), gray (pending)
- Works for your own applications or by manual ID entry

### 📅 Appointment Booking System
- Month calendar view with date selection
- Time slot grid (11 slots per day)
- **Double-booking prevention** — booked slots shown as strikethrough
- Slots locked in localStorage immediately upon booking
- Confirmation triggers push notification to user

### 🔔 Notification System
- Per-user notification feed
- Unread count badge on nav bell icon and sidebar
- Mark individual or all-as-read
- Auto-generated on: submission, approval, appointment confirmation, status changes

### 🤖 AI Fraud Detection
- Every application gets a fraud score (0–100) on submission
- Score bar shown in application detail and admin table
- Color-coded: Green (<30), Amber (30–60), Red (>60)
- High-risk applications show a warning banner

### 📄 PDF Receipt Generator
- Downloads a formatted `.txt` receipt with application details, fee, appointment, and reference ID
- Available on every application detail page

### 🛡️ Admin Panel

**Overview Dashboard:**
- 4 KPI cards (total, pending, approved, users)
- Status distribution with mini progress bars
- High-priority action queue (pending applications)
- Full applications table with one-click Approve / Reject / Dispatch

**All Applications Page:**
- Filterable by status
- Fraud score bars per application
- Action buttons update status + generate user notification instantly

**User Management:**
- All registered citizens with verification status and application count

**Analytics Dashboard:**
- Weekly application volume bar chart
- Normal vs Tatkal split with percentage bars
- Status distribution pie breakdown
- Center-wise load grid (8 PSK centers)

---

## 🗂️ Pages & Routes

Routing is handled by a `currentPage` string variable and a central `render()` function. No URL routing library needed.

| Page Key | Route | Access |
|----------|-------|--------|
| `landing` | `/` | Public |
| `login` | `/login` | Public |
| `register` | `/register` | Public |
| `forgot` | `/forgot` | Public |
| `dashboard` | `/dashboard` | User |
| `new_app` | `/apply` | User |
| `my_apps` | `/applications` | User |
| `view_app` | `/application/:id` | User |
| `track` | `/track` | User |
| `appointments` | `/appointments` | User |
| `notifications` | `/notifications` | User |
| `profile` | `/profile` | User |
| `admin` | `/admin` | Admin only |
| `admin_apps` | `/admin/applications` | Admin only |
| `admin_users` | `/admin/users` | Admin only |
| `analytics` | `/admin/analytics` | Admin only |

---

## 🎨 Design System

### Typography
- **Headings / Titles:** EB Garamond (serif) — authoritative, government feel
- **Body / UI:** DM Sans — clean, modern, accessible
- **Code / IDs:** DM Mono — application reference numbers

### Color Palette

| Name | Hex | Used For |
|------|-----|----------|
| Navy `--navy` | `#0a1628` | Primary brand, headers, sidebar |
| Gold `--gold` | `#c9a84c` | Accents, CTA buttons, highlights |
| Cream `--cream` | `#faf8f3` | Page background |
| Success | `#1a6b3c` | Approved status |
| Warning | `#7a5200` | Pending/review status |
| Danger | `#8b1a1a` | Rejection, errors |
| Info | `#1a4a7a` | Informational alerts |

### Component Library (built-in)
- Buttons: `.btn`, `.btn-primary`, `.btn-gold`, `.btn-outline`, `.btn-ghost`, `.btn-danger`, `.btn-success`
- Badges: `.badge-success`, `.badge-warning`, `.badge-danger`, `.badge-info`, `.badge-gold`, `.badge-gray`
- Cards: `.card`, `.stat-card`
- Forms: `.form-input`, `.form-select`, `.form-label`, `.form-helper`, `.form-error`
- Alerts: `.alert-success`, `.alert-danger`, `.alert-warning`, `.alert-info`
- Timeline: `.timeline`, `.timeline-item`, `.timeline-dot`
- Step Progress: `.step-header`, `.step-item`, `.step-circle`
- Upload Zone: `.upload-zone`, `.upload-preview`
- Calendar: `.calendar-grid`, `.cal-day`, `.time-slot`

---

## 🔧 How to Customize

### Add a New Page

```javascript
// 1. Add to the render() switch
case 'my_new_page': html = `${renderGovtBanner()}${renderNavbar()}${pageMyNewPage()}`; break;

// 2. Create the page function
function pageMyNewPage() {
  return `
    <div class="layout">
      ${renderSidebar()}
      <main class="main-content animate-fade">
        <div class="page-header">
          <h1 class="page-title">My New Page</h1>
        </div>
        <!-- your content -->
      </main>
    </div>`;
}

// 3. Add to sidebar (in renderSidebar())
{ page: 'my_new_page', icon: 'file', label: 'My Page' }
```

### Add a New DB Collection

```javascript
// In DB.init()
if (!this.get('my_collection')) {
  this.set('my_collection', []);
}

// Read
const items = DB.get('my_collection') || [];

// Write
DB.set('my_collection', items);
```

### Change Application Statuses

Edit the `adminAction()` function and the `statusBadge()` map:

```javascript
function statusBadge(s) {
  const map = {
    your_status: ['badge-info', '● Your Label'],
    // ...
  };
}
```

---

## 🚀 Deployment

### Option 1: GitHub Pages (Free)
```bash
# 1. Create a new GitHub repo
# 2. Upload index.html and README.md
# 3. Go to Settings → Pages → Source: main branch
# 4. Your site: https://yourusername.github.io/npdps/
```

### Option 2: Netlify (Free, Instant)
1. Go to [netlify.com](https://netlify.com)
2. Drag and drop the `NPDPS/` folder onto the deploy zone
3. Live in under 30 seconds

### Option 3: Vercel (Free)
```bash
npm install -g vercel
cd NPDPS/
vercel
```

### Option 4: Just double-click
Open `index.html` directly in Chrome, Firefox, Edge, or Safari. No server needed.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML5, CSS3, JavaScript (ES6+) |
| Styling | Custom CSS with CSS Variables (no framework) |
| Fonts | Google Fonts (EB Garamond + DM Sans + DM Mono) |
| Storage | Browser localStorage (simulated backend DB) |
| Routing | Custom SPA router (string-based, zero dependencies) |
| Build Tool | None — zero build step |
| Dependencies | None — zero npm packages |
| Bundle Size | ~1 file, ~3,300 lines |

---

## 📈 What Makes This "Production-Grade"

| Feature | Implementation |
|---------|---------------|
| Role-based access | Admin vs User pages with redirect guards |
| Session persistence | localStorage-based JWT simulation |
| Form validation | Field-level checks before step progression |
| Auto-save draft | Saved on every input change |
| Double-booking prevention | Slot registry checked before confirming |
| Audit trail | Status changes logged with timestamp and officer note |
| Real-time notifications | Generated on every system event |
| Fraud detection | Score computed at submission, shown to admin |
| Responsive UI | Works on mobile, tablet, and desktop |
| Animated transitions | CSS keyframe animations on page changes |
| Toast notifications | Non-blocking feedback for every action |
| Government authenticity | Marquee notices, emblem, official typography |

---

## 📝 Resume Points

Add this to your resume or portfolio:

> **National Passport Digital Processing System (NPDPS)**
> Full-stack government portal simulation — Single-page application built from scratch using vanilla HTML/CSS/JS with zero dependencies. Features JWT-simulated authentication, role-based access control (citizen vs admin), 5-step intelligent application form with auto-save, real-time status tracking, appointment booking with double-booking prevention, AI fraud detection scoring, admin approval workflow, analytics dashboard, and PDF receipt generation. Data persisted via a custom localStorage DB layer simulating a RESTful backend.

---

## 📞 Support

This is an educational/portfolio project and is not affiliated with the Government of India, Ministry of External Affairs, or the official Passport Seva portal.

For real passport applications, visit: [passportindia.gov.in](https://passportindia.gov.in)

---

*Built with ❤️ as a production-quality portfolio project. NPDPS — National Passport Digital Processing System.*
