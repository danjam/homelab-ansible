# LabOps Notification Guide

LabOps supports multiple notification methods to keep you informed about maintenance operations, backups, and system alerts.

## Available Notification Methods

### Email Notifications

Email notifications send detailed information about operations to your specified email address.

**Configuration:**
```bash
# In labops.conf
SMTP_SERVER="smtp.gmail.com"
SMTP_PORT="587"
SMTP_USER="your-email@gmail.com"
SMTP_PASSWORD="your-app-password"
NOTIFICATION_EMAIL="admin@example.com"
```

**Note:** If using Gmail, you'll need to create an App Password.

### Webhook Notifications

Webhook notifications can integrate with various services like Slack, Discord, or custom systems.

**Configuration:**
```bash
# In labops.conf
NOTIFICATION_WEBHOOK_URL="https://hooks.example.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX"
NOTIFICATION_WEBHOOK_AUTH="Bearer your-token" # Optional
```

### Telegram Notifications

Telegram notifications send messages to a user or group chat via a Telegram bot.

**Configuration:**
```bash
# In labops.conf
TELEGRAM_BOT_TOKEN="your-bot-token"  # From @BotFather
TELEGRAM_CHAT_ID="your-chat-id"      # User or group chat ID
TELEGRAM_SILENT_NOTIFICATION="false" # Set to true for silent notifications
```

## Setting Up Telegram Notifications

1. **Create a Telegram Bot:**
   - Open Telegram and search for `@BotFather`
   - Send the command `/newbot`
   - Follow the instructions to create your bot
   - Save the token provided (e.g., `123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ`)

2. **Get your Chat ID:**
   - For personal notifications:
     - Search for `@userinfobot` on Telegram
     - Start a chat and it will display your ID (e.g., `123456789`)
   - For group notifications:
     - Add your bot to the group
     - Send a message in the group mentioning the bot
     - Access https://api.telegram.org/bot{YOUR_BOT_TOKEN}/getUpdates
     - Find the "chat" object and copy the "id" value (usually negative for groups)

3. **Configure LabOps:**
   - Edit your `labops.conf` file
   - Add the following lines:
     ```
     TELEGRAM_BOT_TOKEN="your-bot-token"
     TELEGRAM_CHAT_ID="your-chat-id"
     ```

4. **Test the Notification:**
   - Run a test operation like:
     ```bash
     ./labops.sh --tags healthcheck
     ```
   - You should receive a notification in Telegram

## Notification Format

All notification methods receive similar content:

- **Subject:** Operation type and host name
- **Time:** Timestamp of the operation
- **Host:** The server name and IP address
- **Message:** Details about the operation
- **Status:** Success or failure information

## Multiple Notification Methods

You can configure multiple notification methods simultaneously. LabOps will send notifications to all configured channels.
