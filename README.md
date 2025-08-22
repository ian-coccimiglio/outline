# Outline Project - Local Setup & Verification Report

- **Project:** Outline (`https://github.com/chiillbro/outline`)
- **Original by:** Siva Sankar Reddy Asam
- **Updated by:** Ian Coccimiglio
- **Last Updated Date:** August 21, 2025

---

## Verdict

**This project is fully self-contained and can be run locally end-to-end on macOS, Linux and Windows (via WSL 2).**

This forked repo can be cloned and run without requiring any external API keys or secrets. All necessary services are managed via Docker Compose, and the application correctly falls back to a first-run setup screen, allowing for local user provisioning.

---

## 1. Prerequisites

Before starting, you must have the following tools installed and configured on your system.

### **For Windows Users: A Note on WSL 2**

This project uses a Linux-based development environment. To run it successfully on Windows, it is **highly recommended** to use the Windows Subsystem for Linux **WSL 2**.

1. Open PowerShell **as an Administrator** and run: `wsl --install`
2. This will install WSL and the default Ubuntu distribution. Once complete, restart your machine.
3. You can now open the "Ubuntu" app from your Start Menu. **All subsequent commands in this guide should be run inside this Ubuntu terminal**.

_Please feel free to check tech blogs, Web or Youtube videos for clear and visual instructions._

### **Available software**

1. **Docker Desktop:** download it from the official website (or Docker with Docker Compose)
2. **Node.js (v18+) & Yarn(v1)**
3. **`make` command-line tool**
4. **`mkcert`** **(for generating locally trusted development certificates)**

> [!IMPORTANT]
> After installing `mkcert` for the first time, you must run this one-time command to install the local certificate authority into your system and browser trust scores `mkcert -install`

---

## 2. Step-by-Step Setup Instructions

1. **Clone the Repo:**

```bash
git clone https://github.com/chiillbro/outline.git
```

2. **Configure Environment Variables:**
   1. Create a `.env` file by copying the sample: `cp .env.sample .env`
   2. Open the new `.env` file and make the following critical edits:
      1. **Set the URL:**

      ```bash
      URL=https://localhost:3000
      ```

      2. **Generate Secret Keys (run `openssl rand -hex 32` in the terminal for two times)**

      ```bash
      SECRET_KEY=[your first random string]
      UTILS_SECRET=[your second random string]
      ```

      3. **Ensure local file storage is enabled:**

      ```bash
      FILE_STORAGE=local
      FILE_STORAGE_LOCAL_ROOT_DIR=./data
      ```

      4. **Disable ALL external auth providers** by commenting them out with a `#`. This is critical for reaching the first-run setup screen.

      ```bash
      # Comment out these variables in `.env`
      # SLACK_CLIENT_ID=...
      # SLACK_CLIENT_SECRET=...
      # SLACK_VERIFICATION_TOKEN=...
      # SLACK_APP_ID=...
      # SLACK_MESSAGE_ACTIONS=...
      ```

> [!NOTE]
>
> 1. Please ensure the `FILE_STORAGE_LOCAL_ROOT_DIR` was changed to `./data` for security reasons
> 2. As per my testing, Commenting out Slack variables was enough. Please feel free to comment out other auth provider variables if necessary


3. **Start the Application:**
   - From your terminal, inside the cloned outline project directory, run:

   ```bash
   make up
   ```

   - This single command will start the Docker services, install dependencies, create SSL certs, run database migrations automatically and start the development server. Leave this terminal window running.

---

## 3. End-to-End Verification

The following steps were successfully completed:

- **Application Starts:** The application runs and is accessible at `https://localhost:3000`
- **First-Run Provisioning:** The application correctly displays a setup screen to create the first workspace and admin user.
- **User Signup:** Able to create a new user account through the initial setup screen.
- **Core Functionality:** Able to log in, create a new collection, and create a new document.
- **File Uploads:** Confirmed that uploading an image to a document stores the file in the `./data` directory within the project folder

---

## 4. Notes

- No significant blockers were found. The project is well-architected for local development across all major platforms, provided Windows users utilize WSL 2.
- The `make up` command in the `Makefile` handles the entire setup process, including database migrations, which simplifies the documented steps.
