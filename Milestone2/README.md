FreightQuote AI
**AI-powered freight quoting platform with secure multi-user access**
FastAPI · SQLite · JWT · RBAC · Google Colab · ngrok
## 📑 Table of Contents

1. [Overview](#-overview)
2. [Application Preview](#-application-preview)
3. [Features](#-features)
4. [Tech Stack](#-tech-stack)
5. [Project Structure](#-project-structure)
6. [How to Run](#-how-to-run)
7. [Authentication](#-authentication)
8. [Roles and Privileges (RBAC)](#-roles-and-privileges-rbac)
9. [Admin Dashboard](#-admin-dashboard)
10. [AI Copilot](#-ai-copilot)
11. [Design](#-design)
12. [Remaining Screenshots](#-remaining-screenshots)
13. [Security Notes](#-security-notes)
14. [Team Contributions](#-team-contributions)

---

## 📖 Overview

FreightQuote AI is a multi-user freight-quoting platform. It ships with an **SQL-only AI Copilot** and **six background agents**: Route, Spot Quotes, Carriers, Weather Risk, Margin and Customs.

In this milestone the provided single-administrator app was turned into a real multi-user product:

- Milestone 1 login, signup and forgot-password flow ported to FastAPI
- Profile page for every user
- Five roles with server-side Role-Based Access Control
- Admin dashboard for user management
- Improved AI Copilot
- Refreshed, responsive design with dark mode
  ## 🖼️ Application Preview

### AI Copilot

Answers come from real database queries. Quick-question chips help users get started.

![AI Copilot page](Screenshots/Screenshot%202026-11-08%20145843.jpg)

### Route AI (Agent 1)

Shows the planned route for every shipment with weather risk, route, cost, transit days and the reason for the choice. The Re-route button reruns the planner.

![Route AI page](Screenshots/Screenshot%202026-11-08%20145612.jpg)

### Spot Quotes (Agent 2)

Lists every generated quote with customer, lane, container, carrier, price and creation date.

![Spot Quotes page](Screenshots/Screenshot%202026-11-08%20145911.jpg)

### Admin Dashboard: Users

KPI cards, an Add user form and the users table with role drop-downs and Lock / Delete actions. Only the Admin role can open this page.

![Admin dashboard users page](Screenshots/Screenshot%202026-11-08%20150115.jpg)

### Profile

Shows username, email, role, joined date and last login. Users can change their username and password.

![Profile page](Screenshots/Screenshot%202026-11-08%20150144.jpg)

## ✨ Features

| Area | What it does |
|---|---|
| 🔐 Authentication | Signup, login, forgot/reset password, JWT sessions, lock after 5 wrong passwords |
| 👤 Profile | View account details; change username and password |
| 🛡️ RBAC | Five roles; permissions enforced on the server (HTTP 403) |
| 🧑‍💼 Admin dashboard | Add, delete, promote/demote, lock/unlock users; activity log; KPI cards |
| 🤖 AI Copilot | SQL-only answers with the query and row count shown |
| 🚚 Agents | Route, Spot Quotes, Carriers, Weather Risk, Margin, Customs on a background worker |
| 🎨 Design | Dark mode switch, responsive layout, loading states, clear errors |

---

## 🧰 Tech Stack

| Layer | Technology |
|---|---|
| Backend | FastAPI (Python) |
| Database | SQLite (`sqlite3`) |
| Auth | JWT (PyJWT), bcrypt / PBKDF2 |
| Frontend | Single-page app (`static/app.js`, `static/style.css`) |
| Hosting | Google Colab + ngrok |

---

## 📂 Project Structure

| File | Purpose |
|---|---|
| `server.py` | FastAPI app: `/api/me`, `/api/page/{id}`, `/api/detail/{id}`, `/api/copilot`, `/api/run_cycle`, auth and admin routes |
| `rbac.py` | `ROLE_MENU` and `ROLE_CAPS` dictionaries, the single source of truth for permissions |
| `views_core.py`, `views_freight.py` | Build each page from blocks (KPIs, tables, charts) |
| `analyst.py`, `qa.py`, `intent_router.py` | SQL Copilot: intents and whitelisted SQL |
| `live_engine.py`, `worker.py` | Background worker running agent cycles and refreshing weather |
| `static/app.js`, `static/style.css` | Front end: navigation, login, blocks, Copilot chat, themes |
| `Milestone2_FreightQuote_FastAPI.ipynb` | Notebook that launches the whole app in Colab |

> `localuser.py` (the single local administrator) was removed once real authentication was added.

---

## 🚀 How to Run

1. Open `Milestone2_FreightQuote_FastAPI.ipynb` in **Google Colab**.
2. Add the secrets below in the Colab **Secrets** panel (key icon in the left sidebar).
3. Choose **Runtime → Run all**.
4. Wait for the cell to print `FreightQuote AI is running`.
5. Click the **Open the app** link printed by the last cell.
6. If Colab blocks the link, add `NGROK_AUTHTOKEN` as a secret and re-run the last cell. It prints an ngrok link instead.

### Colab Secrets

> ⚠️ Never type a password, token or API key into a notebook cell. Read secrets with `userdata.get("NAME")`.

| Secret name | Used for |
|---|---|
| `JWT_SECRET` | Signing login tokens |
| `NGROK_AUTHTOKEN` | Fallback public link |
| `EMAIL_ADDRESS` | Sender Gmail for OTP mails |
| `EMAIL_PASSWORD` | Gmail app password |

---

## 🔐 Authentication

**Routes**

| Method | Route | Purpose |
|---|---|---|
| POST | `/api/auth/signup` | Create an account |
| POST | `/api/auth/login` | Log in and receive a JWT |
| POST | `/api/auth/forgot` | Request a password reset OTP |
| POST | `/api/auth/reset` | Reset the password |

**Behaviour**

- Users live in the SQLite `users` table: email, username, password hash, role, locked, created_at, customer_id.
- A JWT is issued on login and sent in the `Authorization: Bearer` header on every API call.
- `user_of(request)` reads the token, checks the user exists and is **not locked**, and returns the real user. The lock flag is checked on every request, so a locked user's token stops working immediately.
- When `/api/me` returns 401, the front end shows the login form. A **Sign out** button is in the top bar.
- An unknown email and a wrong password return the **same error text**.
- The account is **locked after 5 wrong passwords**.

**Validation rules (from Milestone 1)**

- Email: at least 2 letters before `@`, 2 between `@` and the dot, 2 after the dot
- Password: at least 8 characters with upper case, lower case, number and special character
- Confirm password must match

---

## 🛡️ Roles and Privileges (RBAC)

| Role | Who is this | Privileges |
|---|---|---|
| **Admin** | Platform operator | Everything: all pages, create quotes, inject storms, manage users and roles, view the activity log |
| **Operations Manager** | Runs logistics | Route, Carriers, Weather, Customs, Copilot; may run agents and re-route; cannot manage users |
| **Freight Broker** | Sells and prices | Copilot, Quotes (create and view), Carriers, Margin; cannot inject storms or edit customs |
| **Auditor** | Compliance reviewer | Read-only access to every page; cannot create quotes or run agents |
| **Customer** | External shipper | Copilot (only their own shipments) and Quotes for their own account; no other pages |

### Page Access Matrix

| Role | Copilot | Route | Quotes | Carriers | Weather | Margin | Customs | Run agents | Users |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Operations Mgr | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ |
| Freight Broker | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| Auditor | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Customer | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

### How it is enforced

- `rbac.py` holds `ROLE_MENU = {role: set(page ids)}` and `ROLE_CAPS = {role: set(capability names)}`.
- `menu_for(user)` returns only the pages the role may open.
- `need(ctx, capability)` raises **HTTP 403** when the role lacks the capability. It is called in every action route.
- Enforcement is **server-side**: typing `/api/page/<id>` directly also returns 403. Hiding a menu item is not security.
- **Customers** see only their own quotes; quote pages are filtered by `users.customer_id`.
- The AI Copilot reads only data the user's role may see.

---

## 🧑‍💼 Admin Dashboard

Only the **Admin** role can open this page. Every route behind it calls `need(ctx, "users")`.

| Feature | Details |
|---|---|
| KPI cards | Total users, users per role, locked users |
| Users table | Email, username, role, status (Active / Locked), created, last login |
| Add user | Email, username, temporary password, role, with the same validation rules as signup |
| Delete user | With a confirmation step |
| Promote / demote | Change a user's role from a drop-down |
| Lock / Unlock | Locked users cannot log in; existing tokens stop working immediately |
| Activity log | Each admin action (who, what, when); latest 50 rows shown |

**Safety rules**

- An admin cannot delete or lock their own account.
- The last remaining Admin can never be removed.
- Passwords and hashes are never shown.

---

## 🤖 AI Copilot

The Copilot uses **no RAG and no LLM**. It handles greetings and gives exact answers from whitelisted SQL queries. Every number comes from a database query.

**Enhancements**

- At least 15 new question patterns (intents in `analyst.py` or whitelisted queries in `qa.py`)
- Tolerates spelling mistakes and different wordings of the same question
- Shows the SQL behind each answer and the number of rows
- Replies politely to out-of-scope questions and suggests three valid ones
- Respects RBAC: only data the role may see is queried

**Starter questions (shown as chips in the app)**

- Show shipments with weather risks.
- How many quotes are generated?
- Which ports have high congestion?
- Show the delayed shipments.
- What is the average margin?
- Show all carriers.

**New question examples**

- Which carrier is cheapest to Rotterdam?
- How many shipments are on customs hold?
- Show quotes below 10% margin.
- Which ports are most congested?
- Show storms near the Red Sea.

---

## 🎨 Design

- Consistent colour theme with a **dark mode switch**
- Readable tables with clear spacing
- Responsive layout for phones
- Loading states and clear error messages
- Carefully designed login, profile and admin pages

---

## 📸 Remaining Screenshots

The five screenshots above are done. These still need to be captured and added to the `screenshots/` folder:

| Status | Screenshot | File name |
|:-:|---|---|
| ⬜ | Login page | `screenshots/login.png` |
| ⬜ | Signup page | `screenshots/signup.png` |
| ⬜ | Denied (403) action for a restricted role | `screenshots/forbidden_403.png` |
| ⬜ | Allowed action for a permitted role | `screenshots/allowed.png` |
| ⬜ | Copilot answering an exact question and a greeting | `screenshots/copilot_answer.png` |
| ⬜ | Weather page after a storm | `screenshots/weather_storm.png` |

To show one in this README, add a line like this under the matching section:

```html
<p align="center"><img src="screenshots/login.png" alt="Login page" width="100%"></p>
```

---

## 🔒 Security Notes

- No secrets, `.db` files or tokens are committed to the repository.
- All notebook outputs are cleared before submission.
- Passwords are hashed and hashes are never displayed.
- Secrets are kept only in Colab Secrets.
- Permissions are enforced on the server for every page and action.

---

## 👥 Team Contributions

| Team Member | Contribution |
|---|---|
| **Abhinaya Sama** | Authentication and Profile Management |
| **Gaikwad Samruddhi Ashok** | Role-Based Access Control |
| **Vadher Krishnaben Kanjibhai** | Admin Dashboard and User Management |
| **Yesaswi Sushma Peela** | AI Copilot Enhancement and UI Design |

