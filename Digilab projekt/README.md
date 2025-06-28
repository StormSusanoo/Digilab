# School Project: Discord Bot & DigiLab Integration

## Project Overview

This project demonstrates the integration of two different technologies - a Discord bot and a DigiLab (Node-RED) system - to create a unified monitoring solution that tracks and displays system uptime.

## Project Goals

**Primary Objective:** Create a connected system where my Discord bot can communicate with my DigiLab to display real-time uptime information.

**Learning Outcomes:**
- Understanding API integration between different platforms
- Implementing real-time data communication
- Creating user-friendly interfaces for system monitoring
- Demonstrating practical IoT and automation concepts

## Technical Implementation

### Components Used
1. **Discord Bot** - Acts as the user interface and command processor
2. **DigiLab (Node-RED)** - Handles uptime tracking and data processing
3. **API Communication** - Connects both systems for data exchange

### How It Works
1. The DigiLab continuously tracks its own runtime
2. When a user types a command in Discord (like `!uptime`)
3. The Discord bot makes an API request to the DigiLab
4. The DigiLab responds with current uptime data
5. The Discord bot displays this information in a user-friendly format

### Expected Output
Users will be able to see information such as:
- How long the DigiLab has been running
- System status and health
- Last restart time
- Connection status between systems

## Educational Value

This project demonstrates:
- **System Integration** - Connecting different platforms through APIs
- **Real-world Applications** - Similar to how industrial monitoring systems work
- **Problem Solving** - Handling network communication and error management
- **User Experience** - Creating intuitive interfaces for technical data

## Project Deliverables

1. Working Discord bot with uptime commands
2. DigiLab flow that tracks and serves uptime data
3. Documentation of the integration process
4. Demonstration of the system in action

This project showcases practical application of programming concepts, API development, and system integration skills in a real-world scenario.