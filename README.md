# FW/1 + CommandBox + Tailwind Starter Project

This project is a lightweight ColdFusion (CFML) application using:

- **FW/1 (Framework One)** for MVC structure
- **CommandBox** for local development
- **TailwindCSS** for styling

This project reflects a working configuration. The following reflects the steps taken (after much debugging and troubleshooting) to get a working project.

---

## 📁 Folder Structure

```
fw1tw/
  .gitignore
  .vscode/
  box.json            ← CommandBox config
  node_modules
  public/             ← Webroot (deployed to a server or use CommandBox)
    Application.cfc
    controllers/
    css/              ← Tailwind CSS is generated to this location
    framework/        ← Framework One
    index.cfm
    layouts/
    models/
    services/
    views/
  README.md
  server.json         ← CommandBox server config
  src/
  css/                ← Tailwind source files (NOT deployed)
      app.css         ← Main Tailwind CSS file
  tailwind.config.js  ← Tailwind Config
  tools/              ← Build configs (optional)
```

### Why this structure works

- `Application.cfc` must be in the **webroot** so Lucee can detect and load it.
- FW/1 must also be **inside the webroot**, otherwise Lucee will not load it.

---

## ⚙️ server.json

Working CommandBox configuration:

```json
{
  "name": "fw1tw",
  "background": true,
  "web": {
    "http": {
      "port": 8080
    },
    "webroot": "public",
  }
}
```

---

## 🧱 1. Create the Project Directory

```
mkdir myproject
cd myproject
```

---

## 📦 2. Install FW/1 Using CommandBox

```
box install fw1
```

This installs FW/1. There is no default project structure on a fresh install so we have to create that too.

---

## 🗂️ 3. Create the Folder Structure

In the project root folder:

```
mkdir public
mkdir src
mkdir src\css
mkdir tools
```

In the public folder:

```
mkdir controllers
mkdir css
mkdir layouts
mkdir models
mkdir services
mkdir views
```

---

## 🧩 4. Create `Application.cfc` and Use Standard FW/1 Bootstrapping

Creat a new file in the app directory called Application.cfc, and copy this code into the file:

```cfml
component extends="framework.one" {

    this.name = "fw1tw";
    this.mappings = {
    };
    variables.framework = {
        defaultSection = "main",
        defaultItem = "default"
    };

}
```

This is the **standard** FW/1 initialization pattern.

---

## 🌐 5. Create the Front Controller in `/public`

Create:

```
public/index.cfm
```

Add:

```cfml
<!-- FW/1 auto-boots. This file intentionally left blank. -->
```

FW/1 will automatically route requests based on your Application.cfc.

---

## 🎨 6. Create Tailwind Source File

Create:

```
src/css/app.css
```

Add:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## 🧰 7. Initialize Node and Install Tailwind

```
npm init -y
npm install -D tailwindcss @tailwindcss/cli
```

Create `tailwind.config.js` in the project root with the following code:

```js
module.exports = {
  content: ['./app/views/**/*.cfm'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

---

## 🧵 8. Build Tailwind Output into `/public/css`

Run Tailwind:

```
npx @tailwindcss/cli -i ./src/css/app.css -o ./public/css/app.css --watch
```

---

## 🧹 9. Create a `.gitignore`

Add the following rules:

```
# Project specific files/directories to ignore
public/css/app.css

#Standard .gitignore for ColdFusion projects follows
WEB-INF/*
.project
settings.xml
.DS_Store

# Logs
logs/
*.log

# Environment-specific or temporary files
config.json
*.swp
*.bak
*.temp

# NPM modules directory (if using Node.js in the project)
node_modules/

# OS generated files
.DS_Store
.cache/
.idea/
Thumbs.db
```

---

## ⚙️ 10. Create `server.json` and Set Port to 8080

Create:

```
server.json
```

Add:

```json
{
  "name": "fw1tw",
  "background": true,
  "web": {
    "http": {
      "port": 8080
    },
    "webroot": "public"
  }
}
```

This ensures CommandBox serves from `/public` on port **8080**.

---

## 🚀 11. Start the Server

```
box server start
```

Visit:

```
http://localhost:8080
```

Your FW/1 app should now load using the standard FW/1 boot process and the modern folder structure.

---

## 🔄 12. Restarting the Server

```
box server restart
```
