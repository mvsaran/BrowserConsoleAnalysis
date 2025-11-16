# 🔍 BrowserConsoleAnalysis

> A lightweight utility for capturing, analyzing, and reporting browser console messages using Playwright MCP and GitHubCopilot

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

## 💻 Usage

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
│   └── amazon_console_report.md
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

| Website | Report |
|---------|--------|
| Flipkart | [`reports/flipkart_console_report.md`](reports/flipkart_console_report.md) |
| Amazon | [`reports/amazon_console_report.md`](reports/amazon_console_report.md) |

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

1. 🍴 Fork the repository
2. 🌿 Create a feature branch
3. 💬 Submit a pull request

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

1. 🍴 Fork the repository
2. 🌿 Create a feature branch
3. 💬 Submit a pull request

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


<div align="center">
  
**Made with ❤️ using Playwright**

⭐ Star this repo if you find it helpful!

</div>
