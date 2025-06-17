# Discord Bot Setup Guide 
## Table of Contents
- [Prerequisites](#prerequisites)
- [Step 1: Create Your Discord Application](#step-1-create-your-discord-application)
- [Step 2: Set Up Your Development Environment](#step-2-set-up-your-development-environment)
- [Step 3: Create Your Bot Code](#step-3-create-your-bot-code)
- [Step 4: Invite Your Bot to a Server](#step-4-invite-your-bot-to-a-server)
- [Step 5: Run Your Bot](#step-5-run-your-bot)
- [Common Issues & Troubleshooting](#common-issues--troubleshooting)
- [Next Steps](#next-steps)
- [Resources](#resources)

## Prerequisites

Before you begin, make sure you have:
- A Discord account
- Basic knowledge of JavaScript (recommended but not required)
- A computer with internet access
- About 30-45 minutes of free time

## Step 1: Create Your Discord Application

### 1.1 Access the Discord Developer Portal
1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Log in with your Discord account
3. Click the **"New Application"** button in the top right

### 1.2 Configure Your Application
1. Enter a name for your bot (you can change this later)
2. Accept Discord's Terms of Service
3. Click **"Create"**

### 1.3 Create the Bot User
1. In your application dashboard, click **"Bot"** in the left sidebar
2. Click **"Add Bot"** button
3. Confirm by clicking **"Yes, do it!"**

### 1.4 Get Your Bot Token
1. Under the "Token" section, click **"Copy"** to copy your bot token
2. ** IMPORTANT**: Keep this token secret! Never share it publicly or commit it to version control

## Step 2: Set Up Your Development Environment

### 2.1 Install Node.js
1. Download Node.js from [nodejs.org](https://nodejs.org/) (choose LTS version)
2. Install Node.js following the installer instructions
3. Verify installation by opening terminal/command prompt and running:
   ```bash
   node --version
   npm --version
   ```

### 2.2 Create Your Project Directory
```bash
mkdir my-discord-bot
cd my-discord-bot
```

### 2.3 Initialize Your Project
```bash
npm init -y
```

### 2.4 Install Discord.js
```bash
npm install discord.js
```

### 2.5 Create Environment File
1. Create a file named `.env` in your project folder
2. Add your bot token:
   ```
   DISCORD_TOKEN=your_bot_token_here
   ```
3. Install dotenv to manage environment variables:
   ```bash
   npm install dotenv
   ```

## Step 3: Create Your Bot Code

### 3.1 Create the Main Bot File
Create a file named `index.js` and add this basic bot code:

```javascript
// Load environment variables
require('dotenv').config();

// Import Discord.js
const { Client, GatewayIntentBits } = require('discord.js');

// Create a new client instance
const client = new Client({ 
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ] 
});

// When the client is ready, run this code once
client.once('ready', () => {
    console.log(`Bot is online! Logged in as ${client.user.tag}`);
});

// Listen for messages
client.on('messageCreate', message => {
    // Ignore messages from bots
    if (message.author.bot) return;

    // Respond to "hello" messages
    if (message.content.toLowerCase() === 'hello') {
        message.reply('Hello there!');
    }

    // Simple ping command
    if (message.content.toLowerCase() === '!ping') {
        message.reply('Pong!');
    }
});

// Login to Discord with your bot token
client.login(process.env.DISCORD_TOKEN);
```

### 3.2 Update Package.json
Add a start script to your `package.json`:

```json
{
  "scripts": {
    "start": "node index.js"
  }
}
```

## Step 4: Invite Your Bot to a Server

### 4.1 Generate Invite Link
1. Go back to your Discord Developer Portal
2. Click on your application, then **"OAuth2"** → **"URL Generator"**
3. Under **"Scopes"**, select:
   - `bot`
   - `applications.commands` (for slash commands)
4. Under **"Bot Permissions"**, select:
   - `Send Messages`
   - `Read Message History`
   - `Use Slash Commands`
   - Any other permissions your bot needs

### 4.2 Invite the Bot
1. Copy the generated URL at the bottom
2. Open the URL in your browser
3. Select the server you want to add the bot to
4. Click **"Authorize"**
5. Complete the captcha if prompted

## Step 5: Run Your Bot

### 5.1 Start Your Bot
In your terminal, run:
```bash
npm start
```

If everything is set up correctly, you should see:
```
Bot is online! Logged in as YourBotName#1234
```

### 5.2 Test Your Bot
Go to your Discord server and try:
- Type `hello` - the bot should respond with "Hello there!"
- Type `!ping` - the bot should respond with "Pong!"

## Common Issues & Troubleshooting

### Bot Not Responding
- **Check permissions**: Ensure the bot has permission to read and send messages in the channel
- **Verify token**: Make sure your bot token is correct in the `.env` file
- **Check intents**: Ensure you have the correct Gateway Intents enabled

### "Invalid Token" Error
- Double-check your bot token in the `.env` file
- Make sure there are no extra spaces or characters
- Regenerate the token in the Developer Portal if needed

### Bot Appears Offline
- Check that your code is running without errors
- Verify the bot is invited to the server with proper permissions
- Look for error messages in your terminal

### Permission Errors
- Ensure the bot's role is positioned high enough in the server's role hierarchy
- Check that required permissions are granted when inviting the bot

## Next Steps

Now that your bot is running, you can expand its functionality:

### Add Slash Commands
```javascript
// Example slash command setup
const { SlashCommandBuilder } = require('discord.js');

// Register commands (you'll need additional setup for this)
```

### Add More Features
- Music playback
- Moderation commands
- Custom responses
- Database integration
- Web dashboard

### Host Your Bot
Consider hosting your bot on:
- **Heroku** (free tier available)
- **Railway** (simple deployment)
- **DigitalOcean** (VPS hosting)
- **Your own server**

## Project Structure
```
my-discord-bot/
├── node_modules/
├── .env
├── .gitignore
├── index.js
├── package.json
└── package-lock.json
```

## Important Security Notes

1. **Never share your bot token** - treat it like a password
2. **Add `.env` to `.gitignore`** if using version control:
   ```
   # .gitignore
   .env
   node_modules/
   ```
3. **Use environment variables** for sensitive data
4. **Regularly regenerate tokens** if compromised

## Resources

- **Official Discord.js Guide**: [discordjs.guide](https://discordjs.guide/)
- **Discord.js Documentation**: [discord.js.org](https://discord.js.org/)
- **Discord Developer Portal**: [discord.com/developers](https://discord.com/developers)
- **Node.js Documentation**: [nodejs.org/docs](https://nodejs.org/docs/)