# Car Monitor Telegram Bot

A Telegram bot that monitors newly posted car listings on Ukrainian marketplaces (**OLX** and **AutoRia**) based on user-defined filters and sends notifications when new cars appear.

---

## Features

- 🔍 Searches for cars on:
  - **OLX**
  - **AutoRia**
- ⏱ Detects only **new listings** (posted within the last 15 minutes)
- 🤖 Telegram bot interface for user interaction
- 🗄 Stores user preferences in a **MySQL database**
- 🔁 Automatically checks for new cars every 15 minutes
- ⚙ User-configurable filters:
  - Price range
  - Production year range

---

## How It Works

1. A user starts the bot in Telegram.
2. The user sets:
   - Year range (e.g. `2005-2018`)
   - Price range (e.g. `3000-8000`)
3. These parameters are saved in a MySQL database.
4. Every 15 minutes:
   - The bot fetches latest car listings from OLX and AutoRia.
   - Filters listings by price, year, and publish time.
   - Sends matching cars directly to the user via Telegram.

---

## Technologies Used

- **Python 3**
- **Telegram Bot API** (`pyTelegramBotAPI`)
- **MySQL**
- **BeautifulSoup (bs4)** – HTML parsing
- **Requests** – HTTP requests
- **schedule** – task scheduling
- **threading** – background bot polling

---

## Project Structure

```text
.
├── main.py
├── config.ini
└── README.md
```

---

## Configuration

Create a `config.ini` file in the root directory:

```ini
[admin]
username = your_mysql_username
password = your_mysql_password
token = your_telegram_bot_token
```

---

## Database Schema

Example `users` table:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id BIGINT NOT NULL UNIQUE,
    price_from INT NOT NULL,
    price_to INT NOT NULL,
    year_from INT NOT NULL,
    year_to INT NOT NULL
);
```

---

## Bot Commands

| Command        | Description |
|---------------|------------|
| `/start`      | Start the bot and check saved preferences |
| `/setparams`  | Set or update car filters |
| `/messages`   | (Admin only) Start scheduled notifications |

---

## Admin Features

- Only the admin (by Telegram ID) can enable scheduled checks using `/messages`
- Notifications are sent every **15 minutes**

---

## Limitations

- HTML structure changes on OLX or AutoRia may break parsing
- No async HTTP requests (can be optimized)
- No error handling for Telegram rate limits

---

## Possible Improvements

- Use async (`aiohttp`, `aiogram`)
- Add car brand and model filters
- Store already sent listings to avoid duplicates
- Dockerize the application
- Add logging instead of `print()`

---

## Disclaimer

This project is for educational purposes. Scraping websites may violate their terms of service. Use responsibly.

Created as a learning project for Telegram bots, web scraping, and backend integration with MySQL.
