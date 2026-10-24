# Milestone 1 – Maritime Freight Pricing and Route Optimization

## 1. Project Overview

Milestone 1 focuses on developing a secure authentication system for the Maritime Freight Pricing and Route Optimization project. It provides a user-friendly interface for account registration, login, password recovery, and administrator access.

The system uses Python and Streamlit to provide the user interface, SQLite to store user account information, and authentication mechanisms to help protect user credentials and manage sessions.

## 2. Features Implemented

- **User Registration:** Allows new users to create an account.
- **User Login:** Authenticates users using their credentials.
- **Password Security:** Uses bcrypt hashing to protect user passwords.
- **Forgot Password:** Supports password recovery using security questions and email OTP verification.
- **Email OTP Verification:** Sends a one-time password to the registered email address.
- **OTP Expiration and Lockout:** Handles expired OTPs and limits verification attempts.
- **Admin Login:** Provides a separate login interface for administrators.
- **SQLite Database:** Stores user account information.
- **Session Management:** Uses JWT-based authentication to manage user sessions.
- **Secrets Management:** Uses Google Colab Secrets to manage sensitive configuration values securely.

## 3. Technology Stack

- **Programming Language:** Python
- **Web Framework:** Streamlit
- **Database:** SQLite
- **Password Hashing:** bcrypt
- **Authentication:** PyJWT (JWT)
- **Email Verification:** SMTP
- **Development Environment:** Google Colab
- **Version Control:** Git and GitHub

## 4. Project Files

The `Milestone1` folder contains the following files and resources:

- `README.md` – Project overview, features, technology stack, and setup instructions.
- `Milestone1_Auth.ipynb` – Jupyter notebook containing the authentication system code.
- `Milestone 1 — User Authentication Module.pdf` – Milestone documentation.
- `screenshots/` – Screenshots demonstrating the implemented features.

**Note:** Update the notebook filename in this section if your actual filename is different.

## 5. How to Run the Notebook

Follow these steps to run the authentication system.

1. Open [Google Colab](https://colab.research.google.com/).
2. Upload or open the `Milestone1_Auth.ipynb` notebook.
3. Configure the required secrets in the Colab Secrets panel.
4. Install the required Python libraries if they are not already installed.
5. Run the notebook cells in the correct order.
6. Start the Streamlit application using the commands provided in the notebook.
7. If public access is required, configure ngrok and its authentication token as described in the notebook.
8. Open the generated application URL.
9. Test the signup, login, forgot password, email OTP verification, and admin login features.

**Important:** The application may not work correctly if required secrets, dependencies, or public-access settings are missing or incorrectly configured.

## 6. Secrets Configuration (Google Colab)

The application uses Google Colab Secrets to store sensitive credentials and configuration values. These values support email OTP delivery, password recovery, and JWT-based authentication.

### 6.1 Required Secrets

Configure the following secrets according to the variables used in your notebook.

| Secret Name | Purpose |
|---|---|
| `EMAIL_ADDRESS` | Gmail address used to send OTP emails. |
| `EMAIL_PASSWORD` | Gmail App Password used for SMTP authentication. |
| `JWT_SECRET` | Secret key used to sign and verify JWT authentication tokens. |
| `ADMIN_USERNAME` | Username for administrator login. |
| `ADMIN_PASSWORD` | Password for administrator login. |
| `PROJECT_LINK` | Link to the project repository or application, if used by the code. |
| `NGROK_AUTH_TOKEN` | Authentication token used to configure ngrok, if public tunneling is required. |

### 6.2 How to Configure Colab Secrets

1. Open the notebook in Google Colab.
2. Click the **Secrets (key icon)** in the left sidebar.
3. Add each required secret using its exact variable name.
4. Enter the corresponding value for each secret.
5. Enable notebook access for the required secrets.
6. Run the notebook cells that load the secrets before starting the application.

**Note:** The secret names must match the names expected by the notebook code. Add only the secrets required by your implementation.

### 6.3 Gmail App Password Setup

1. Enable two-step verification on your Google account.
2. Open your Google Account security settings.
3. Generate a Gmail App Password.
4. Store your Gmail address in `EMAIL_ADDRESS`.
5. Store the generated App Password in `EMAIL_PASSWORD`.
6. Ensure the notebook uses the correct Gmail SMTP configuration.

The Gmail App Password allows the application to authenticate with Gmail SMTP and send OTP verification emails.

### 6.4 ngrok Configuration

If the application needs to be accessible through a public URL, configure ngrok as required by the notebook.

1. Create or sign in to your ngrok account.
2. Obtain your ngrok authentication token.
3. Store the token securely in Colab Secrets using the variable name expected by your code.
4. Run the ngrok setup instructions in the notebook.
5. Start Streamlit and verify that the generated public URL opens the application.

Do not publish your ngrok authentication token in the repository or screenshots.

### 6.5 Security Best Practices

- Never hardcode passwords or API tokens directly into source code.
- Never upload credentials, Gmail App Passwords, JWT secrets, or ngrok tokens to GitHub.
- Use Google Colab Secrets to manage sensitive values.
- Keep the `JWT_SECRET` private and use a strong, unpredictable value.
- Do not share screenshots that expose secret values.
- If a credential is accidentally exposed, revoke or replace it immediately.

**Security Note:** Colab Secrets help protect credentials from being stored directly in the notebook. Access permissions and safe handling of credentials are still important.

## 7. Screenshots

The following screenshots demonstrate the user authentication features implemented in Milestone 1.

### 7.1 Signup Page

![Signup Page](screenshots/Screenshot%202026-10-24%20210710.png)

### 7.2 Login Page

![Login Page](screenshots/Screenshot%202026-10-24%20210632.png)

### 7.3 Forgot Password and OTP Verification

![Forgot Password](screenshots/Screenshot%202026-10-24%20210741.png)

![OTP Verification](screenshots/Screenshot%202026-10-24%20210802.png)

### 7.4 Admin Login

![Admin Login](screenshots/Screenshot%202026-10-24%20210827.png)

**Note:** The image links assume that the screenshot files are uploaded to the `screenshots` folder with exactly these filenames. GitHub filenames are case-sensitive, so ensure the paths and filenames match your repository.

## 8. Conclusion

Milestone 1 establishes the authentication foundation for the Maritime Freight Pricing and Route Optimization project. It includes user registration, secure login, password recovery, email OTP verification, administrator access, and JWT-based session management.

The project uses Python, Streamlit, SQLite, bcrypt, PyJWT, SMTP, and Google Colab Secrets to support authentication and credential management.

This milestone provides a foundation for future project development and additional maritime freight pricing and route optimization features.
