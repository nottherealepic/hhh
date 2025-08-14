# Discord Giveaway Bot — Render-Ready Template

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/adarshpothan/epic-giveaway-discord-bot)  
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  
[![Python 3.13.4](https://img.shields.io/badge/python-3.13.4-3776AB.svg?logo=python)](https://www.python.org/)  
[![discord.py](https://img.shields.io/badge/discord.py-v2.3.2-7289DA.svg?logo=discord&logoColor=white)](https://github.com/Rapptz/discord.py)  
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791.svg?logo=postgresql&logoColor=white)](https://www.postgresql.org/)


A feature-rich, production-friendly Discord giveaway bot built with **Python 3.13**, **discord.py**, and **PostgreSQL**. Designed to run out-of-the-box on **Render** with minimal setup.

> ![Bot Showcase GIF](https://i.ibb.co/vxT603mq/Sequence01-1-ezgif-com-video-to-gif-converter.gif)

---

## 📋 Table of Contents

- [About The Project](#about-the-project)
- [Key Features](#-key-features)
- [Built With](#-built-with)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Local Installation](#local-installation)
- [Deploying to Render](#-deploying-to-render)
- [Configuration](#-configuration)
- [Usage](#-usage)
  - [Slash Commands](#slash-commands)
  - [Setting Up the Uptime Panel](#setting-up-the-uptime-panel-optional)
- [Database Schema](#-database-schema)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## About The Project

This project is a complete, deployable template for a Discord giveaway bot. It provides administrators with powerful slash commands to create, manage, and automatically conclude giveaways.

The backend is powered by **PostgreSQL** for persistent storage, and the entire application is architected for seamless deployment on **Render's** free-tier background workers.

## ✨ Key Features

- **🎁 Admin-Only Slash Commands:** Securely manage giveaways.
- **🐘 Robust PostgreSQL Backend:** Persists all giveaway and participant data.
- **⚙️ Auto-Managed Database:** Automatically creates and indexes required tables on first run.
- **🔒 Flexible Role Permissions:** Control who can run commands via a simple environment variable (`ADMIN_ROLES`).
- **🚀 One-Click Render Deployment:** Go from zero to a running bot in minutes.
- **📊 Optional Uptime Panel:** Display bot status and server name in a dedicated channel.

## 🛠️ Built With

This project leverages these incredible open-source libraries and platforms:

*   [Python 3.13.4](https://www.python.org/)
*   [discord.py](https://github.com/Rapptz/discord.py)
*   [asyncpg](https://github.com/MagicStack/asyncpg) (PostgreSQL Driver)
*   [pytz](https://pypi.org/project/pytz/) (Timezone Formatting)
*   [Render](https://render.com/) (Hosting Platform)

---

## 🚀 Getting Started

Follow these steps to get a local copy up and running for development or to deploy your own instance.

### Prerequisites

*   A **Discord Bot Application** created in the [Discord Developer Portal](https://discord.com/developers/applications).
    *   Copy the **Bot Token**.
    *   Under the *Bot* tab, enable the **Message Content Intent**.
*   A **PostgreSQL 16** database. (Render provides one for free.)

### Local Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/adarshpothan/epic-giveaway-discord-bot.git
    cd epic-giveaway-discord-bot
    ```
2.  **Create and activate a virtual environment:**
    ```bash
    # Create the virtual environment
    python -m venv .venv

    # Activate it
    # Windows
    .venv\Scripts\activate
    # macOS/Linux
    source .venv/bin/activate
    ```
3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
4.  **Configure your environment:**
    *   Copy the example `.env` file: `cp .env.example .env`
    *   Edit the `.env` file with your own credentials (Bot Token, Database URL, etc.).
5.  **Run the bot:**
    ```bash
    python bot.py
    ```

## ☁️ Deploying to Render

Deploying is the easiest way to get this bot running 24/7.

1.  **Push your code to a GitHub repository.**
2.  Click the **Deploy to Render** button at the top of this README.
3.  In Render's dashboard:
    *   Create a **PostgreSQL** instance. Render will automatically link it and provide the `DATABASE_URL`.
    *   Create a **Background Worker**.
    *   **Runtime:** `Python 3`
    *   **Start Command:** `python bot.py`
    *   **Environment:** Fill in your environment variables. Render will pre-fill `DATABASE_URL`.
4.  Click **Create Background Worker**. Check the **Logs** tab to see the bot come online.

## 🔧 Configuration

The bot is configured via environment variables.

| Variable              | Description                                                                                                                               | Default      |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| `SERVER_NAME`         | Name shown in the uptime embed title.                                                                                                     | `MyServer`   |
| `BOT_TOKEN`           | **Required.** Your Discord bot token.                                                                                                     | `None`       |
| `DATABASE_URL`        | **Required.** PostgreSQL connection string (`postgresql://user:pass@host/dbname`).                                                        | `None`       |
| `STATUS_CHANNEL_ID`   | Channel ID for the uptime embed. Set to `0` to disable.                                                                                   | `0`          |
| `UPTIME_MSG_ID`       | Message ID of the uptime embed to edit. Set to `0` to disable.                                                                            | `0`          |
| `ADMIN_ROLES`         | Comma-separated, case-insensitive list of roles that can use admin commands (e.g., `Admin,Moderator`). If empty, only server admins can. | `Admin,Mod`  |

---

## 💡 Usage

### Slash Commands

| Command           | Description                                                        | Parameters                                                                                             |
| ----------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| `/epicgiveaway`   | Starts a new giveaway. (Admin Only)                                | `title`, `sponsor`, `duration` (minutes), `item`, `winners` (count), `channel`                         |
| `/view`           | Views the first 20 rows from a database table. (Admin Only)        | `tablename` (e.g., `giveaways` or `participants`)                                                      |
| `/dt`             | Lists all public tables in the database. (Admin Only)              | *None*                                                                                                 |
| `/get_msg_id`      | Sends a dummy message to the `uptime_status_channel_id` and returns the message ID.          | *None*                                                                                                 |

**Example Giveaway:**
`/epicgiveaway title:"Nitro August" sponsor:"@user" duration:60 item:"Discord Nitro (1 Month)" winners:2 channel:#giveaways`

### Setting Up the Uptime Panel (Optional)

1.  In your desired status channel, run the `/get_msg_id` command.
2.  The bot will post a placeholder message and reply with its **Message ID**.
3.  Go to your Render environment variables:
    *   Set `STATUS_CHANNEL_ID` to the ID of the channel you used.
    *   Set `UPTIME_MSG_ID` to the message ID you just copied.
4.  Re-deploy the bot. It will now find and update that message periodically.

---

## 🗄️ Database Schema

The bot automatically creates and manages the following PostgreSQL tables.

<details>
<summary>Click to view schema</summary>

**`giveaways` table**
```sql
CREATE TABLE IF NOT EXISTS giveaways (
    id SERIAL PRIMARY KEY,
    message_id BIGINT NOT NULL,
    channel_id BIGINT NOT NULL,
    end_time TIMESTAMPTZ NOT NULL,
    prize VARCHAR(255) NOT NULL,
    winners_count INTEGER NOT NULL DEFAULT 1,
    host_id BIGINT,
    ended BOOLEAN NOT NULL DEFAULT FALSE
);
```

**`participants` table**
```sql
CREATE TABLE IF NOT EXISTS participants (
    giveaway_id INTEGER REFERENCES giveaways(id) ON DELETE CASCADE,
    user_id BIGINT NOT NULL,
    PRIMARY KEY (giveaway_id, user_id)
);
```

**Indexes** are also created on `(ended, end_time)` and `(giveaway_id)` for performance.
</details>

## 🩺 Troubleshooting

*   **`relation "giveaways" does not exist`**: Ensure the bot's logs show "Database tables ensured." and verify your `DATABASE_URL` is correct.
*   **Slash commands not visible**: Make sure you invited the bot to your server with the `applications.commands` scope enabled.
*   **Permission denied on commands**: Confirm your role is listed in the `ADMIN_ROLES` environment variable (case-insensitive) or that you have server administrator permissions.

---

## 🤝 Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

## 📜 License & Philosophy

This project was built to empower individuals and communities. The goal is to provide a powerful, ready-to-host giveaway bot that anyone can deploy with minimal effort, using environment variables for all customization so you never have to touch the code.

To support this goal, this project is released under the permissive **MIT License**.

In simple terms, this means you are free to:

*   ✅ **Use:** Run this bot for your personal or community servers, free of charge.
*   ✅ **Modify:** Change the code to add your own features or tweaks.
*   ✅ **Distribute:** Share your modified version with others.
*   ✅ **Use Commercially:** You can even use this bot as part of a commercial project.

The only real condition is that you must include the original copyright and license notice in any copy or substantial portion of the software. It's provided "as is" without warranty.

For the full legal text, please see the [LICENSE](LICENSE) file.
