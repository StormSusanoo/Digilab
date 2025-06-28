# How to Add APIs to Node-RED - Simple Guide

## What This Does
Connect your Node-RED flows to live data from the internet (weather, news, APIs, etc.) and display or use that data in your automations.

## Prerequisites  
- Node-RED is installed and running
- You can access the Node-RED editor (usually at http://localhost:1880)
- You have an API key from some service (optional for free APIs)
- 10-15 minutes

## Step 1: Basic API Call Flow

### 1.1 Create Your First Flow
1. **Open Node-RED editor** in your browser
2. **Drag these nodes** from the left panel:
   - `inject` node (under input)
   - `http request` node (under network) 
   - `debug` node (under output)

### 1.2 Connect the Nodes
Connect them in order: `inject` → `http request` → `debug`

### 1.3 Configure the HTTP Request Node
Double-click the `http request` node and set:
- **Method**: GET
- **URL**: `https://api.thecatapi.com/v1/images/search`
- **Return**: a parsed JSON object
- **Name**: Get Cat Picture

Click **Done**.

### 1.4 Configure Debug Node
Double-click the `debug` node:
- **Output**: complete msg object
- **Name**: API Response

Click **Done**.

### 1.5 Test It
1. Click **Deploy** (top right)
2. Click the button on the `inject` node
3. Check the **Debug** tab (right panel) - you should see cat picture data!

## Step 2: Weather API (With API Key)

### 2.1 Get API Key
1. Go to https://openweathermap.org/api
2. Sign up for free account
3. Get your API key

### 2.2 Create Weather Flow
Drag these nodes:
- `inject` node
- `function` node (under function)
- `http request` node  
- `debug` node

Connect: `inject` → `function` → `http request` → `debug`

### 2.3 Configure Function Node
Double-click the `function` node and add this code:
```javascript
// Set the city you want weather for
const city = "London";
const apiKey = "YOUR_API_KEY_HERE";  // Replace with your actual key

// Build the API URL
msg.url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

return msg;
```

Name it "Build Weather URL" and click **Done**.

### 2.4 Configure HTTP Request
- **Method**: GET
- **URL**: Leave empty (we set it in function)
- **Return**: a parsed JSON object

### 2.5 Test Weather API
1. **Deploy** your flow
2. Click the `inject` button
3. Check debug output - you should see weather data!

## Step 3: Make It Interactive with Dashboard

### 3.1 Install Dashboard (if not installed)
1. Go to **Menu** → **Manage palette**
2. Click **Install** tab
3. Search for `node-red-dashboard`
4. Click **Install**

### 3.2 Create Dashboard Flow
Add these nodes:
- `text input` (under dashboard)
- `function` node
- `http request` node
- `template` node (under dashboard)

### 3.3 Configure Text Input
- **Group**: Create new group called "Weather"
- **Label**: City Name
- **Name**: City Input

### 3.4 Configure Function Node
```javascript
const city = msg.payload;
const apiKey = "YOUR_API_KEY_HERE";

if (!city) {
    msg.payload = "Please enter a city name";
    return msg;
}

msg.url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

return msg;
```

### 3.5 Configure Template Node
```html
<div style="padding: 20px; font-family: Arial;">
    <h2>Weather in {{payload.name}}</h2>
    <p><strong>Temperature:</strong> {{payload.main.temp}}°C</p>
    <p><strong>Feels like:</strong> {{payload.main.feels_like}}°C</p>
    <p><strong>Description:</strong> {{payload.weather[0].description}}</p>
    <p><strong>Humidity:</strong> {{payload.main.humidity}}%</p>
</div>
```

### 3.6 View Your Dashboard
1. **Deploy** the flow
2. Go to `http://localhost:1880/ui`
3. Enter a city name and see the weather!

## Step 4: Easy APIs You Can Try

### Random Joke API
```javascript
// In function node:
msg.url = "https://official-joke-api.appspot.com/random_joke";
return msg;
```

### Random Quote API  
```javascript
// In function node:
msg.url = "https://api.quotable.io/random";
return msg;
```

### Dog Pictures API
```javascript
// In function node:
msg.url = "https://dog.ceo/api/breeds/image/random";
return msg;
```

### Number Facts API
```javascript
// In function node:
const number = 42; // or get from input
msg.url = `http://numbersapi.com/${number}`;
return msg;
```

## Step 5: Handle API Keys Securely

### 5.1 Use Environment Variables
Instead of putting API keys in your code:

1. **Stop Node-RED**
2. **Set environment variable**:
   - Windows: `set WEATHER_API_KEY=your_key_here`
   - Mac/Linux: `export WEATHER_API_KEY=your_key_here`
3. **Restart Node-RED**

### 5.2 Use Environment Variable in Function
```javascript
const apiKey = env.get('WEATHER_API_KEY');
const city = msg.payload;

msg.url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

return msg;
```

## Complete Weather Dashboard Flow

Here's what your final flow should look like:

```
[Text Input] → [Function] → [HTTP Request] → [Template]
                    ↓
               [Debug Node]
```

**Function Node Code:**
```javascript
const city = msg.payload;
const apiKey = env.get('WEATHER_API_KEY') || "YOUR_API_KEY_HERE";

if (!city || city.trim() === '') {
    msg.payload = {error: "Please enter a city name"};
    return msg;
}

msg.url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

return msg;
```

## Tips

### Do This 
- Always test with simple APIs first (like cat pictures)
- Use the debug node to see what data you're getting
- Check the API documentation for correct URL format
- Use environment variables for API keys
- Add error handling in your function nodes

### Don't Do This 
- Put API keys directly in function nodes
- Forget to set HTTP request to return "parsed JSON object"
- Make too many API calls too quickly
- Ignore the debug output when something's not working

## Common Issues

**"Cannot read property of undefined"**: The API response structure is different than expected. Check debug output.

**"Request failed"**: Check your API URL and make sure the API key is correct.

**Dashboard not showing**: Make sure you deployed the flow and the dashboard nodes are in the same group.

**Empty response**: Some APIs require headers or authentication. Check the API documentation.