 FreightQuote AI
AI-Powered Freight Quoting Platform with Secure Multi-User Access

FastAPI · SQLite · JWT Authentication · RBAC · Google Colab · ngrok

Infosys Springboard Internship 7.0 | Batch 4 | Milestone 2

📑 Table of Contents
Overview
Application Preview
Features
Tech Stack
Project Structure
How to Run
Authentication
Roles and Privileges (RBAC)
Admin Dashboard
AI Copilot
Design
Remaining Screenshots
Security Notes
Team Contributions
📖 Overview

FreightQuote AI is an AI-powered, multi-user freight-quoting platform designed to simplify freight operations, shipment planning, carrier selection, and quote management.

The application includes an SQL-only AI Copilot and six background agents:

Route AI
Spot Quotes
Carriers
Weather Risk
Margin Analysis
Customs
Milestone 2 Enhancements

The existing single-administrator application was upgraded into a secure multi-user platform with:

Authentication system with signup, login, and forgot-password functionality.
Individual profile management for every user.
Five user roles with server-side Role-Based Access Control (RBAC).
Dedicated Admin Dashboard for user management.
Enhanced SQL-based AI Copilot.
Improved user interface with responsive design and dark mode.
Secure JWT-based authentication and session management.
🖼️ Application Preview
1. AI Copilot

Provides accurate answers using real database queries. Includes quick-question suggestions to help users interact with shipment and freight data.

!AI Copilot

2. Route AI (Agent 1)

Displays optimized shipment routes with weather risks, estimated costs, transit days, and route-selection explanations.

The Re-route button allows users to rerun the route planner.

!Route AI

3. Spot Quotes (Agent 2)

Displays generated freight quotes with customer information, shipping lanes, container details, carriers, pricing, and creation dates.

!Spot Quotes

4. Admin Dashboard – User Management

Provides administrative controls, including:

KPI cards and user statistics.
Add User functionality.
User management table.
Role assignment and modification.
Lock and unlock accounts.
Delete user functionality.

Only users with the Admin role can access this dashboard.

!Admin Dashboard

5. Profile Management

Displays user account information, including username, email, role, registration date, and last login.

Users can update their usernames and passwords.

!Profile

✨ Features
Module	Description
🔐 Authentication	Signup, login, forgot/reset password, JWT sessions
👤 Profile Management	View account details and update username/password
🛡️ RBAC	Five roles with server-side permission enforcement
🧑‍💼 Admin Dashboard	Add, delete, promote, demote, lock, and unlock users
📊 Activity Log	Tracks administrative actions
🤖 AI Copilot	SQL-based answers with query and row count
🚚 Route AI	Shipment route planning and re-routing
💰 Spot Quotes	Freight quote generation and management
🚛 Carriers	Carrier information and analysis
🌦️ Weather Risk	Weather-related shipment risk monitoring
📈 Margin Analysis	Freight margin analysis
📦 Customs	Customs-related shipment information
🎨 UI/UX	Responsive design, dark mode, loading states, and error messages
🧰 Tech Stack
Layer	Technology
Backend	FastAPI (Python)
Database	SQLite
Authentication	JWT (PyJWT)
Password Security	bcrypt / PBKDF2
Frontend	HTML, CSS, JavaScript
AI Copilot	Python-based SQL query engine
Background Processing	Python worker
Development Environment	Google Colab
Public Access	ngrok
📂 Project Structure
text
FreightQuote-AI/
│
├── Milestone2_FreightQuote_FastAPI.ipynb
│
├── server.py
├── rbac.py
├── views_core.py
├── views_freight.py
│
├── analyst.py
├── qa.py
├── intent_router.py
│
├── live_engine.py
├── worker.py
│
├── static/
│   ├── app.js
│   └── style.css
│
├── screenshots/
│   ├── ai_copilot.png
│   ├── route_ai.png
│   ├── spot_quotes.png
│   ├── admin_users.png
│   ├── profile.png
│   ├── login.png
│   ├── signup.png
│   ├── forbidden_403.png
│   ├── allowed.png
│   ├── copilot_answer.png
│   └── weather_storm.png
│
└── README.md

Important Files
File	Purpose
server.py	FastAPI application, authentication, API endpoints, and admin routes
rbac.py	Centralized role and permission definitions
views_core.py	Core application page construction
views_freight.py	Freight-related page construction
analyst.py	AI Copilot intent recognition
qa.py	Whitelisted SQL queries and answers
intent_router.py	Routes user questions to appropriate intents
live_engine.py	Background agent execution engine
worker.py	Background worker for agent cycles and weather updates
static/app.js	Frontend application logic
static/style.css	Application styling
Milestone2_FreightQuote_FastAPI.ipynb	Google Colab notebook to launch the application
🚀 How to Run

The application is designed to run using Google Colab and ngrok.

Step 1: Open Google Colab

Open the notebook:

Milestone2_FreightQuote_FastAPI.ipynb

Upload it to Google Colab or open it directly from your repository.

Step 2: Configure Colab Secrets
Open the Secrets panel in Google Colab.
Add the required secrets.
Enable notebook access to each secret.
Secret Name	Purpose
JWT_SECRET	Signs authentication tokens
NGROK_AUTHTOKEN	Provides an alternative public application link
EMAIL_ADDRESS	Gmail sender address for OTP emails
EMAIL_PASSWORD	Gmail application password

Important: Never hardcode passwords, API keys, or tokens inside notebook cells. Read sensitive values using userdata.get("NAME").

Step 3: Run the Notebook

Select:

Runtime → Run all

Wait for the notebook to initialize the FastAPI application.

Step 4: Access the Application

When the application starts successfully, the notebook displays:

FreightQuote AI is running

Click the generated Open the app link.

If the default Colab link does not work, configure NGROK_AUTHTOKEN in Colab Secrets and rerun the final cell to generate an ngrok link.

🔐 Authentication

FreightQuote AI provides secure multi-user authentication using JWT.

Authentication Routes
Method	Endpoint	Purpose
POST	/api/auth/signup	Create a new user account
POST	/api/auth/login	Authenticate and receive JWT
POST	/api/auth/forgot	Request password-reset OTP
POST	/api/auth/reset	Reset account password
Authentication Workflow
Users register using their email, username, and password.
The application validates the registration details.
Passwords are securely hashed before storage.
Users log in with their registered credentials.
A JWT token is generated after successful authentication.
The frontend sends the token using the Authorization: Bearer header.
Every protected API request validates the user's identity and account status.
Authentication Security
User information is stored in the SQLite users table.
Passwords are stored as hashes, never as plaintext.
Accounts are locked after five incorrect password attempts.
Locked users cannot access the application, even with previously issued tokens.
Unknown email and incorrect password attempts return the same error message.
Users can securely sign out.
Password Validation Rules
Minimum 8 characters.
At least one uppercase letter.
At least one lowercase letter.
At least one number.
At least one special character.
Confirm password must match.
Email Validation

The registration form validates the email format according to the project's specified rules.

🛡️ Roles and Privileges (RBAC)

The application implements five roles with different levels of access.

User Roles
Role	Description	Privileges
Admin	Platform administrator	Full application access and user management
Operations Manager	Logistics operations	Route, Carriers, Weather, Customs, Copilot, and agent execution
Freight Broker	Freight sales and pricing	Copilot, Quotes, Carriers, and Margin
Auditor	Compliance reviewer	Read-only access to all application pages
Customer	External shipper	Access to Copilot and own freight quotes
Page Access Matrix
Role	Copilot	Route	Quotes	Carriers	Weather	Margin	Customs	Run Agents	Users
Admin	✅	✅	✅	✅	✅	✅	✅	✅	✅
Operations Manager	✅	✅	❌	✅	✅	❌	✅	✅	❌
Freight Broker	✅	❌	✅	✅	❌	✅	❌	❌	❌
Auditor	✅	✅	✅	✅	✅	✅	✅	❌	❌
Customer	✅	❌	✅	❌	❌	❌	❌	❌	❌
How RBAC Works

Role-based access control is implemented in rbac.py.

Two central dictionaries define the access rules:

ROLE_MENU: Defines which application pages each role can access.
ROLE_CAPS: Defines which actions and capabilities each role can perform.

The application uses:

menu_for(user) to display permitted navigation items.
need(ctx, capability) to validate permissions before executing protected actions.
Server-Side Enforcement

Security is not limited to hiding frontend navigation items.

Every protected API route checks the user's permissions.
Unauthorized requests return HTTP 403 Forbidden.
Direct API access is also protected.
Customers can only access their own quotes.
The AI Copilot respects the user's role and data-access permissions.
Auditors have read-only access and cannot execute restricted actions.
🧑‍💼 Admin Dashboard

The Admin Dashboard is exclusively accessible to users with the Admin role.

Every administrative route verifies the users capability.

Dashboard Features
Feature	Description
KPI Cards	Total users, users per role, and locked accounts
User Management	View registered users and their details
Add User	Create accounts with assigned roles
Delete User	Remove users with confirmation
Promote/Demote	Change user roles
Lock/Unlock	Control account access
Activity Log	View the latest 50 administrative actions
Administrative Safety Rules
Admins cannot delete their own accounts.
Admins cannot lock their own accounts.
The last remaining Admin cannot be deleted.
Passwords and password hashes are never displayed.
Administrative actions are recorded in the activity log.
🤖 AI Copilot

The FreightQuote AI Copilot provides database-driven answers to freight-related questions.

Unlike conventional generative AI chatbots, this implementation uses SQL-based question answering without RAG or an LLM.

Key Capabilities
Understands greetings and supported freight-related questions.
Recognizes different wordings and common spelling mistakes.
Supports at least 15 additional question patterns.
Uses whitelisted SQL queries to retrieve actual database information.
Displays the SQL query used to generate each answer.
Shows the number of rows returned.
Respects role-based data access restrictions.
Provides helpful suggestions for unsupported questions.
Starter Questions

Users can try the following questions:

Show shipments with weather risks.
How many quotes are generated?
Which ports have high congestion?
Show the delayed shipments.
What is the average margin?
Show all carriers.
Additional Supported Questions
Which carrier is cheapest to Rotterdam?
How many shipments are on customs hold?
Show quotes below 10% margin.
Which ports are most congested?
Show storms near the Red Sea.
Copilot Workflow
text
User Question
     |
     v
Intent Recognition
     |
     v
Question Classification
     |
     v
RBAC Permission Check
     |
     v
Whitelisted SQL Query
     |
     v
SQLite Database
     |
     v
Query Result Processing
     |
     v
Answer + SQL + Row Count


All numerical answers are derived from database queries rather than fabricated responses.

🎨 Design

FreightQuote AI includes a refreshed and responsive user interface.

Design Improvements
Consistent application color theme.
Dark mode and light mode support.
Responsive layouts for desktop and mobile devices.
Improved tables and dashboard cards.
Clear loading indicators.
User-friendly error messages.
Dedicated login and signup pages.
Improved profile and admin dashboard layouts.
Interactive AI Copilot chat interface.
📸 Remaining Screenshots

The following screenshots are still required to complete the project documentation.

Status	Screenshot	File Name
⬜	Login Page	screenshots/login.png
⬜	Signup Page	screenshots/signup.png
⬜	Restricted Role – 403 Forbidden	screenshots/forbidden_403.png
⬜	Permitted Role – Allowed Action	screenshots/allowed.png
⬜	Copilot Exact Answer and Greeting	screenshots/copilot_answer.png
⬜	Weather Page After Storm	screenshots/weather_storm.png
Adding Screenshots

Save the screenshots in the screenshots/ directory.

Use the following Markdown syntax to display an image:

HTML
<p align="center">
  <img src="screenshots/login.png" alt="Login Page" width="100%">
</p>

🔒 Security Notes

Security is an important part of FreightQuote AI.

The following practices are implemented:

Passwords are hashed before being stored.
JWT tokens are used for authenticated API requests.
Account lock status is checked on every protected request.
Server-side RBAC protects pages and actions.
Unauthorized requests receive HTTP 403 responses.
Customer data is restricted to the customer's own account.
SQL Copilot queries are whitelisted.
Sensitive credentials are stored using Google Colab Secrets.
Passwords and password hashes are never exposed in the Admin Dashboard.
No secrets, database files, or authentication tokens should be committed to the repository.
Notebook outputs should be cleared before submission.

Security Reminder: Use strong JWT secrets and protect all application credentials. Public ngrok links should be treated as publicly accessible endpoints.

👥 Team Contributions

The project was collaboratively developed by the following team members.

Team Member	Contribution
Abhinaya Sama -	Authentication and Profile Management
Gaikwad Samruddhi Ashok	- Role-Based Access Control (RBAC)
Vadher Krishnaben Kanjibhai -	Admin Dashboard and User Management
Yesaswi Sushma Peela-	AI Copilot Enhancement and UI Design
