# 🔍 BrowserConsoleAnalysis

> A lightweight utility for capturing, analyzing, and reporting browser console messages using Playwright MCP Server and GitHubCopilot

[![Node.js](https://img.shields.io/badge/node-%3E%3D16-brightgreen.svg)]()
[![Playwright](https://img.shields.io/badge/playwright-latest-orange.svg)]()
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()

---

## 📋 Purpose

Quickly capture console output and runtime errors from web pages for:
- 🐛 **Debugging** — Identify JavaScript errors and warnings
- ✅ **QA Testing** — Automated console monitoring
- 📊 **Performance Analysis** — Track runtime issues
- 🔔 **Monitoring** — Detect production problems early

### 🤖 Powered By

This project leverages modern development tools:
- **Playwright MCP** — Model Context Protocol integration for enhanced browser automation
- **GitHub Copilot** — AI-assisted code development and optimization

---

## ✨ Features

- 🚀 **Quick Setup** — Simple Playwright-based implementation
- 📝 **Markdown Reports** — Human-readable output in `reports/` folder
- 🎯 **Event Capture** — Monitors both `console` and `pageerror` events
- ⏱️ **Runtime Analysis** — Waits for dynamic scripts to execute
- 🔧 **Extensible** — Easy to customize and integrate

---

## 🛠️ What This Does

1. **Launches** a Playwright browser session
2. **Navigates** to the target URL
3. **Listens** for console messages and page errors
4. **Waits** for runtime scripts to complete execution
5. **Generates** a detailed markdown analysis report

### 📊 Sample Output

The tool produces reports like:
- `reports/flipkart_console_report.md`
- `reports/amazon_console_report.md`

---

## 🚀 Quick Start

### Prerequisites

- Node.js (version 16 or higher)
- npm or yarn

### Installation

```powershell
# Initialize project
npm init -y

# Install Playwright
npm install -D playwright

# Install browser binaries
npx playwright install
```

---

## 💻 Implementation Guide

### 🎯 Step-by-Step Implementation

This project uses **Playwright MCP Server** with **GitHub Copilot** in agent mode for intelligent browser console analysis and network monitoring.

#### Step 1: Start Playwright MCP Server

First, ensure the Playwright MCP server is running:

```powershell
# Start the Playwright MCP server
# The server will listen for commands and manage browser automation
playwright-mcp-server start
```

#### Step 2: Open GitHub Copilot in Agent Mode

1. Open VS Code
2. Activate GitHub Copilot
3. Switch to **Agent Mode** (use `@` command in Copilot chat)

#### Step 3: Issue Analysis Prompts

##### 🔍 Example 1: Console Error Analysis

In GitHub Copilot chat, enter:

```
Use playwright MCP Server. Perform analysis of error messages found on console for webpage: https://www.flipkart.com and save the results in markdown file under reports.
```

##### 🌐 Example 2: Network Requests Analysis

For comprehensive network monitoring:

```
Use playwright MCP Server. Run all network requests for website https://www.agoda.com since loading the webpage. Provide full raw list of network requests by breaking them down by type and size. Advise resolutions on possible causes and fixes. Write in a markdown file and attach under reports folder.
```

#### Step 4: Automatic Execution

Once you issue the command, the automation process begins:

**For Console Analysis:**
1. 🌐 **Playwright MCP** opens the browser automatically
2. 🔍 **Navigates** to the target URL (e.g., https://www.flipkart.com)
3. 👂 **Listens** to console messages and page errors
4. ⏳ **Waits** for dynamic content to load
5. 📊 **Analyzes** captured errors and warnings
6. 💾 **Saves** results as markdown in `reports/` folder

**For Network Analysis:**
1. 🌐 **Opens browser** and navigates to target URL (e.g., https://www.agoda.com)
2. 📡 **Captures all network requests** during page load
3. 📊 **Breaks down requests** by:
   - Resource type (img, script, css, xhr, fetch, etc.)
   - Transfer size and byte count
4. 📈 **Generates statistics** (total requests, largest resources)
5. 🔍 **Analyzes performance** and identifies issues
6. 💡 **Provides recommendations** for optimization
7. 💾 **Saves detailed report** as markdown with full resource timing JSON

The entire process is automated through the Playwright MCP server and GitHub Copilot's agent capabilities.

---

## 💻 Manual Usage (Alternative Method)

### Basic Capture Script

Create `capture.js`:

```javascript
const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch({ headless: true });
  const context = await browser.newContext();
  const page = await context.newPage();
  const messages = [];

  // Listen for console messages
  page.on('console', msg => {
    const loc = msg.location ? msg.location() : null;
    messages.push({ 
      source: 'console', 
      level: msg.type(), 
      text: msg.text(), 
      location: loc 
    });
  });

  // Listen for page errors
  page.on('pageerror', err => {
    messages.push({ 
      source: 'pageerror', 
      message: err.message, 
      stack: err.stack 
    });
  });

  // Navigate and capture
  await page.goto('https://www.flipkart.com', { waitUntil: 'networkidle' });
  await page.waitForTimeout(6000); // Allow runtime scripts to execute

  // Output results
  console.log(JSON.stringify(messages, null, 2));
  
  await browser.close();
})();
```

### Run the Script

```powershell
node capture.js
```

---

## 📁 Project Structure

```
BrowserConsoleAnalysis/
├── 📄 README.md
├── 📄 capture.js
├── 📄 package.json
├── 📄 package-lock.json
├── 📂 reports/
│   ├── flipkart_console_report.md
│   ├── amazon_console_report.md
│   ├── agoda_network_requests_report.md
│   └── agoda_resource_timing.json
└── 📂 node_modules/
    └── (dependencies)
```

---

## 🔧 Advanced Usage

### Capture with Network Tracing

```javascript
// Enable tracing
await context.tracing.start({ screenshots: true, snapshots: true });

// ... your capture logic ...

// Save trace
await context.tracing.stop({ path: 'trace.zip' });
```

### Save HAR File

```javascript
const context = await browser.newContext({
  recordHar: { path: 'network.har' }
});
```

---

## 🎯 Extending the Project

### Ideas for Enhancement

- 🖥️ **CLI Wrapper** — Add command-line arguments for URLs and options
- 📦 **HAR Export** — Save network traces for failed resources
- 🔄 **Multi-Run Aggregation** — Detect intermittent errors
- 🌍 **Region Testing** — Test from different locations/user-agents
- 🤖 **CI Integration** — GitHub Actions for periodic monitoring
- 📧 **Alerting** — Email/Slack notifications for critical errors

### Example CLI Extension

```javascript
// Add argument parsing
const url = process.argv[2] || 'https://www.example.com';
const timeout = parseInt(process.argv[3]) || 6000;
```

---

## 📊 Sample Reports

Example reports are included in this repository:

| Website | Type | Report |
|---------|------|--------|
| Flipkart | Console Errors | [`reports/flipkart_console_report.md`](reports/flipkart_console_report.md) |
| Amazon | Console Errors | [`reports/amazon_console_report.md`](reports/amazon_console_report.md) |
| Agoda | Network Analysis | [`reports/agoda_network_requests_report.md`](reports/agoda_network_requests_report.md) |
| Agoda | Resource Timing | [`reports/agoda_resource_timing.json`](reports/agoda_resource_timing.json) |

---

## 📝 Next Steps

- [ ] Add runnable CLI with argument parsing
- [ ] Generate HAR files for network debugging
- [ ] Add automated tests
- [ ] Create GitHub Action workflow
- [ ] Add support for multiple browsers
- [ ] Implement report comparison/diffing

---

## 👨‍💻 Author

**Saran Kumar**

---

## 📄 License

MIT License - feel free to use this project for any purpose!
---

<div align="center">
  
**Made with ❤️ using Playwright**

⭐ Star this repo if you find it helpful!

</div>
