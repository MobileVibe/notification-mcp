# Cline Notification Server

An MCP server for Cline that enables sending notifications and receiving responses via Telegram. When Cline needs to ask a question, it will send you a Telegram message and wait for your response.

## Features

- Send notifications with different urgency levels (low, medium, high)
- Automatically waits for user responses
- Configurable timeout (defaults to 30 seconds)
- Stops Cline when user is unavailable to prevent unwanted actions

## Installation

```bash
npm install cline-notification-server
```

## Setup

### 1. Create a Telegram Bot

1. Message [@BotFather](https://t.me/BotFather) on Telegram
2. Send `/newbot` and follow the instructions
3. Save the bot token you receive (it looks like `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)
4. Start a chat with your new bot
5. Message `/start` to your bot

### 2. Get Your Chat ID

1. Send a message to your bot
2. Open this URL in your browser (replace BOT_TOKEN with your token):
   ```
   https://api.telegram.org/botBOT_TOKEN/getUpdates
   ```
3. Look for the `chat` object and note the `id` field - this is your chat ID

### 3. Configure Cline

Add the server to your Cline configuration file:

For VSCode extension (`cline_mcp_settings.json`):
```json
{
  "mcpServers": {
    "notification": {
      "command": "npx",
      "args": ["cline-notification-server"],
      "env": {
        "TELEGRAM_BOT_TOKEN": "your_bot_token",
        "TELEGRAM_CHAT_ID": "your_chat_id"
      }
    }
  }
}
```

For Claude desktop app (`claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "notification": {
      "command": "npx",
      "args": ["cline-notification-server"],
      "env": {
        "TELEGRAM_BOT_TOKEN": "your_bot_token",
        "TELEGRAM_CHAT_ID": "your_chat_id"
      }
    }
  }
}
```

## How It Works

When Cline needs to ask you a question:
1. It sends a message via Telegram using your bot
2. Waits up to 30 seconds for your response
3. If you respond within 30 seconds, Cline continues with your answer
4. If you don't respond within 30 seconds, Cline stops and waits for you to restart it

## Example Usage

When this server is configured, Cline can use it to ask you questions:

```typescript
// Example of how Cline uses the notification tools
const response = await use_mcp_tool({
  server_name: "notification",
  tool_name: "send_notification",
  arguments: {
    message: "Should I proceed with deploying to production?",
    project: "MyApp",
    urgency: "high"
  }
});
```

Messages are formatted with:
- 🚨 Prefix for high urgency
- ⚠️ Prefix for medium urgency
- No prefix for low urgency

## Environment Variables

- `TELEGRAM_BOT_TOKEN`: Your Telegram bot token from BotFather
- `TELEGRAM_CHAT_ID`: Your Telegram chat ID where messages will be sent

## Contributing

Feel free to open issues or submit pull requests if you have suggestions for improvements.

## License

MIT
