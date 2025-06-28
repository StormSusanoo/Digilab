# How to Add APIs to Your Bot - Simple Guide

## What This Does
Lets your bot get live data from the internet (weather, news, memes, etc.) instead of just responding with pre-written messages.

## Prerequisites
- You already have a working Discord bot
- You have an API key from some service (like weather API)
- 15-20 minutes

## Step 1: Install What You Need

In your bot's folder, run:
```bash
npm install node-fetch
```

This lets your bot make internet requests.

## Step 2: Store Your API Key Safely

### 2.1 Create .env File
In your bot folder, create a file called `.env`:
```
DISCORD_TOKEN=your_discord_bot_token
WEATHER_API_KEY=your_weather_api_key
```

### 2.2 Install dotenv (if you don't have it)
```bash
npm install dotenv
```

### 2.3 Add to .gitignore
Create/edit `.gitignore` file:
```
.env
node_modules/
```

## Step 3: Add API to Your Bot

### 3.1 Basic Structure
At the top of your bot file, add:
```javascript
require('dotenv').config();
const fetch = require('node-fetch');

// Your API key from .env file
const WEATHER_API_KEY = process.env.WEATHER_API_KEY;
```

### 3.2 Create an API Function
Add this function to your bot:
```javascript
async function getWeather(city) {
    try {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${WEATHER_API_KEY}&units=metric`;
        const response = await fetch(url);
        const data = await response.json();
        
        if (response.ok) {
            return ` Weather in ${data.name}: ${data.main.temp}°C, ${data.weather[0].description}`;
        } else {
            return " City not found!";
        }
    } catch (error) {
        return " Something went wrong!";
    }
}
```

### 3.3 Add Bot Command
Add this to your message handler:
```javascript
client.on('messageCreate', async message => {
    if (message.author.bot) return;
    
    // Weather command
    if (message.content.startsWith('!weather ')) {
        const city = message.content.replace('!weather ', '');
        const weatherInfo = await getWeather(city);
        message.reply(weatherInfo);
    }
});
```

## Step 4: Test Your Bot

1. **Run your bot**: `npm start`
2. **In Discord, type**: `!weather London`
3. **Bot should respond with**: `🌤️ Weather in London: 15°C, light rain`

## Complete Example

Here's a full working example:

```javascript
require('dotenv').config();
const { Client, GatewayIntentBits } = require('discord.js');
const fetch = require('node-fetch');

const client = new Client({ 
    intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent] 
});

const WEATHER_API_KEY = process.env.WEATHER_API_KEY;

// Weather function
async function getWeather(city) {
    try {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${WEATHER_API_KEY}&units=metric`;
        const response = await fetch(url);
        const data = await response.json();
        
        if (response.ok) {
            return `🌤️ **${data.name}**\n🌡️ ${data.main.temp}°C (feels like ${data.main.feels_like}°C)\n☁️ ${data.weather[0].description}\n💧 Humidity: ${data.main.humidity}%`;
        } else {
            return " City not found! Try again.";
        }
    } catch (error) {
        return " Weather service unavailable!";
    }
}

client.once('ready', () => {
    console.log(`${client.user.tag} is online!`);
});

client.on('messageCreate', async message => {
    if (message.author.bot) return;
    
    // Weather command
    if (message.content.startsWith('!weather ')) {
        const city = message.content.replace('!weather ', '');
        if (!city) {
            message.reply("Please specify a city! Example: `!weather London`");
            return;
        }
        
        message.reply("🔍 Getting weather data...");
        const weatherInfo = await getWeather(city);
        message.channel.send(weatherInfo);
    }
});

client.login(process.env.DISCORD_TOKEN);
```

## Other Easy APIs to Add

### Random Cat Picture
```javascript
async function getCatPicture() {
    const response = await fetch('https://api.thecatapi.com/v1/images/search');
    const data = await response.json();
    return data[0].url;
}

// In your message handler:
if (message.content === '!cat') {
    const catUrl = await getCatPicture();
    message.reply(catUrl);
}
```

### Random Quote
```javascript
async function getQuote() {
    const response = await fetch('https://api.quotable.io/random');
    const data = await response.json();
    return `"${data.content}" - ${data.author}`;
}

// In your message handler:
if (message.content === '!quote') {
    const quote = await getQuote();
    message.reply(quote);
}
```

## Tips

### Do This 
- Always use `try/catch` for API calls
- Store API keys in `.env` file
- Add `.env` to `.gitignore`
- Handle errors gracefully
- Test with simple commands first

### Don't Do This 
- Put API keys directly in your code
- Forget to handle errors
- Make too many API requests too quickly
- Share your `.env` file

## Common Issues

**"fetch is not defined"**: Install node-fetch with `npm install node-fetch`

**"Invalid API key"**: Check your `.env` file and make sure the key is correct

**Bot doesn't respond**: Make sure you have the `MessageContent` intent enabled

**CORS errors**: These don't happen in Discord bots (only in browsers)

## Next Steps

Once this works, you can:
- Add more APIs (news, jokes, facts)
- Create cooler commands with multiple parameters
- Add slash commands instead of text commands
- Store data in a database
- Add rate limiting to prevent spam