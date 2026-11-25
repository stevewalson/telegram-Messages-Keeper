# Telegram Messages Keeper

The bot logs texts you receive from private chats while you’re offline so you can read them later, get a summary, or clear your history.

Every message is saved locally the moment it arrives. If a sender deletes a message on Telegram, your copy stays put. The bot only catches new messages while it’s running and can’t recover anything from before.

This is just a personal tool. It uses **Secretary Mode** in BotFather to get the necessary chat access, but that’s just a technical setting and not a commercial product.


## What it does

- Receives text messages from chats the account owner has chosen to connect and share with the bot.
- Stores messages and sender details in a local SQLite database named `messages.db`.
- Provides `/assistant` controls to enable/disable message saving, display saved messages in one place, summarize them with AI (Google Gemini), and delete them.
- Does not automatically reply to incoming messages.

Messages are saved only while it is enabled. The database is local to the machine running the bot and remains there until messages are deleted or the database file is removed.

Because a saved copy can remain after the original is deleted in Telegram, use this only in chats where you have the right to retain messages. Be transparent with people you communicate with, follow applicable laws and Telegram's terms, and secure or delete the local archive when it is no longer needed.

## Requirements

- Node.js (use a current LTS release) and npm.
- A Telegram bot created with [@BotFather](https://t.me/BotFather).
- A Google Gemini API key to use the summary feature.

## Create and configure the bot with BotFather

1. Open [@BotFather](https://t.me/BotFather) in Telegram and send `/newbot`.
2. Follow the prompts to choose a display name and a unique username ending in `bot`.
3. Keep the bot token private. If it has ever been shared or committed, revoke it and generate a replacement with BotFather.
4. Open `/mybots`, select your bot, and open **Bot Settings**:
   - Enable **Secretary Mode**. This is Telegram's BotFather setting name for the capability this personal project uses.
   - Disable **Allow Groups** so the bot cannot be added to group chats.
   - Enable **Restrict Bot Usage** , so only you can control the bot. Menu labels may change as Telegram updates BotFather.
5. Open your profile settings and select Chat Automation, and enter the @username of the bot you created and confirm to link it.

## Install and run

1. **Clone the repository**
```bash
   git clone https://github.com/stevewalson/telegram-assistant-dm-forwarder.git
   cd telegram-assistant-dm-forwarder
```
From the project directory:

```sh
npm install
```

### Configure environment variables

update the credentials in the `.env` file :

```dotenv
BOT_TOKEN=your_telegram_bot_token
GEMINI_APIKEY=your_google_gemini_api_key
```

- `BOT_TOKEN` is the token issued by BotFather for your Telegram bot.
- `GEMINI_APIKEY` is the Google Gemini API key used by the summary feature.
- Keep `.env` private and do not commit it to Git or share it. If a token or key has been exposed, revoke it and create a replacement.

Then start the bot:

```sh
node index.js
```

Keep the process running for the bot to receive updates. The bot creates `messages.db` in the current working directory when it starts.

The Gemini key is optional for starting the bot, but it is required to use the **Summary** action. If it is missing, the bot will report that Gemini is not configured.

## Use the bot

1. Open a private chat with the bot and send `/assistant`.
2. Press **Enable** to start collecting eligible incoming messages from connected chats. Press **Disable** to stop.
3. Use **Messages** to view the saved messages, **Summary** to request a Gemini summary, or **Delete Messages** to erase the stored messages.

Only text message content is saved by this version. Summarization sends the saved message text to Google Gemini, so do not use the feature for information you are not allowed to share with that service. Keep the machine and `messages.db` secure, and delete the database when you no longer need its contents.