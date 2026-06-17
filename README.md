# 📚 Interactive Manual Testing Study Guide

A comprehensive, interactive study dashboard designed for Manual Testing QA professionals. This repository contains a complete 12-part reference manual covering every dimension of manual testing—from core SDLC/STLC fundamentals and test design to JIRA workflow execution and modern QA methodologies.

> 🚀 **Live demo:** <a href="https://ghanendra-sdet.github.io/Comprehensive-Manual-Testing-Guide/" target="_blank" rel="noopener noreferrer">View the interactive Manual Testing guide</a>
> 
> Save it to your bookmarks for future learning.

This dashboard converts standard Markdown source files into a premium, responsive single-page web application client-side, making it highly portable and ready to be hosted directly on **GitHub Pages**.

---

## ✨ Features

- **📱 Fully Responsive Design**: Seamless layout transitions between desktop and mobile with modern glassmorphism panels.
- **🌗 Native Dark Mode**: Responsive light/dark theme switching that respects system preferences and matches Mermaid diagram styling.
- **🔍 Full-Text Client-Side Search**: An in-memory, instant search engine that indexes all 12 parts and lets you jump directly to specific sections and headings.
- **📊 Reading Progress Tracker**: Persists completed sections and files in your browser's local storage and displays an overall study progress indicator bar.
- **🎨 Inline Mermaid Diagrams**: Flowcharts and testing models rendered dynamically on the fly.
- **💡 GitHub-Style Alert Callouts**: Custom-designed warning, info, tips, and note panels utilizing vector SVGs.
- **🛠️ Zero Build Steps**: Written entirely in vanilla HTML5, CSS3, and JavaScript—no `npm install` or compilation required.

---

## 📖 Directory Structure

```text
├── index.html                               # Dashboard entry point
├── styles.css                               # Layout, themes, typography, and responsive styles
├── app.js                                   # Routing, search, rendering, and progress logic
├── .nojekyll                                # Prevents GitHub Pages Jekyll build errors
├── README.md                                # Project documentation
├── 00_Table_of_Contents.md                  # Main index of parts
├── Part_01_Software_Testing_Fundamentals.md # QA Fundamentals
├── Part_02_Types_of_Testing.md              # Software Testing Types
├── Part_03_STLC.md                          # Software Testing Life Cycle
├── Part_04_SDLC_Models.md                   # SDLC Frameworks
├── Part_05_Test_Design_Techniques.md        # Black Box Techniques
├── Part_06_Test_Case_Development.md         # Test Writing Templates
├── Part_07_Defect_Bug_Life_Cycle.md         # Bug Lifecycle Metrics
├── Part_08_JIRA_Test_Management.md          # JIRA & JQL Reference
├── Part_09_Agile_Testing.md                 # Scrum, Sprint, Agile QA
├── Part_10_Test_Planning_Documentation.md   # IEEE 829 Test Plans
├── Part_11_Test_Execution_Reporting.md      # Metrics, Execution, TSR
├── Part_12_QA_Best_Practices_Modern_Trends.md # Shift-Left, AI, Careers
└── Resources.md                             # Tools & resources for testers (free/premium/enterprise)
```

---

## 🚀 How to Run Locally

Because the application dynamically fetches the markdown files from the local directory, modern browsers will block these `fetch()` requests due to CORS security policies when files are opened directly via `file://`.

To run the application locally, you must run it through a local web server:

### Option A: VS Code "Live Server" (Recommended)
1. Open this folder in VS Code.
2. Install the **Live Server** extension.
3. Click the **Go Live** button in the bottom right status bar.

### Option B: Node.js (command line)
If you have Node.js installed, run:
```bash
npx serve
```
Then open `http://localhost:3000` in your web browser.

### Option C: Python (command line)
If you have Python installed, run:
```bash
python3 -m http.server 8000
```
Then open `http://localhost:8000` in your web browser.

---

## ☁️ Deploy to GitHub Pages (Hosting)

This project is 100% static, making it perfectly suited for free hosting on GitHub Pages.

### Quick version

1. Create a new repository on GitHub. Example: `manual-testing-study-guide`
2. Push your project files from your computer:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/manual-testing-study-guide.git
   git push -u origin main
   ```
3. Enable GitHub Pages in your repository settings:
   - Open **Settings** on GitHub.
   - Choose **Pages**.
   - Set the source to **Deploy from a branch**.
   - Choose **main** and `/ (root)`.
   - Click **Save**.
4. Open `https://YOUR_USERNAME.github.io/manual-testing-study-guide/`.

> Example: if your username is `ghanendra-sde`, then your Pages URL will be:
> `https://ghanendra-sde.github.io/manual-testing-study-guide/`
>
> If the page does not appear immediately, wait a few minutes and verify that Pages is configured for the `main` branch and `/ (root)` folder.

### Detailed version

This project is 100% static, making it perfectly suited for free hosting on GitHub Pages.

1. **Create a new repository** on GitHub (e.g., `manual-testing-study-guide`).
2. **Push this code** to your repository:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Interactive Manual Testing Study Guide"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/manual-testing-study-guide.git
   git push -u origin main
   ```
3. **Enable GitHub Pages**:
   - Go to your repository settings on GitHub.
   - Select **Pages** from the sidebar.
   - Under **Build and deployment**, set the source to **Deploy from a branch**.
   - Under **Branch**, select `main` and `/ (root)` and click **Save**.
4. Your site will be live at `https://YOUR_USERNAME.github.io/manual-testing-study-guide/` within a couple of minutes!

---

*This guide was compiled as a comprehensive resource for manual testing professionals with 5+ years of experience to master fundamentals, study templates, prep for interviews, and keep up with modern trends.*
