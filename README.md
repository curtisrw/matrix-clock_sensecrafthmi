# Matrix Clock for SenseCraft HMI (reTerminal E1001)

This project provides a customized “Matrix Rain Clock” HTML page designed specifically for the **Seeed reTerminal E1001** running **SenseCraft HMI Cloud**.  
It renders a dense Matrix-style falling code background while embedding the **current time and date** as large, bright “floating” characters inside the rain.

The page is built to suit e‑ink behavior, slow refresh cycles, and the cloud-based rendering delays of SenseCraft HMI.

---

## ⭐ Features

### ✔ Matrix-style falling code  
Dense white-on-black streams with light ghosting, optimized for e‑ink readability.

### ✔ Floating time & date characters  
- Fully **ordered** time/date string (e.g., `3:55 PM  Saturday, Dec 7`)  
- **Each character appears individually** in the Matrix rain  
- Characters are **bold + larger** than rain characters  
- Characters appear to "float" naturally within the rain

### ✔ Timezone correction built-in  
SenseCraft HMI Cloud renders time in UTC+14 for some deployments.  
This code includes:

```js
const OFFSET_HOURS = -14;
```

You may change this to any offset you need.

### ✔ Automatic rounding to nearest 5 minutes  
Since e‑ink refreshes slowly, the displayed time is rounded **down** to the nearest 5 minutes, similar to TRMNL’s aesthetic.

### ✔ Auto-centering and responsive layout  
Works on:
- Full 800×480 reTerminal e‑ink resolution
- Preview mode in SenseCraft HMI
- Desktop browser testing

---

## 📂 File Overview

Only one file is needed:

```
index.html
```

You deploy it as a web page to SenseCraft HMI Cloud, or host it via GitHub Pages / local server.

---

## 🚀 How to Deploy via GitHub Pages

1. Create a new GitHub repository  
2. Add `index.html` to the root  
3. Go to **Settings → Pages**  
4. Set “Build from branch” → `main` → `/ (root)`  
5. Save  
6. GitHub will give you a public URL  

Use this URL as a **Web Page** in your SenseCraft HMI Page List.

---

## ⚙ How It Works (Technical Overview)

### 1. Matrix Rain Loop
A low-opacity fade is applied each frame:

```js
ctx.fillStyle = "rgba(0,0,0,0.06)";
ctx.fillRect(0, 0, canvas.width, canvas.height);
```

This produces the classic vertical ghosting trails.

### 2. Randomized Drop Columns
Each column independently falls at its own rate and resets randomly when leaving the screen.

### 3. Floating Time & Date Characters
Instead of forcing the rain to align with the characters, each time/date character gets:

- A deterministic left‑to‑right order  
- A jittered vertical position  
- Rendered **after** the rain  
- In a larger, bolder font:

```js
ctx.font = "bold " + embedFont + "px monospace";
ctx.fillStyle = "rgba(255,255,255,1)";
```

### 4. Timezone-adjusted timestamp
The script adjusts the runtime clock using:

```js
const adjusted = new Date(now.getTime() + OFFSET_HOURS * 60 * 60 * 1000);
```

Then rounds it to the nearest 5 minutes.

---

## 🔧 Customization

### Change timezone offset
Edit:

```js
const OFFSET_HOURS = -14;
```

### Make floating characters bigger/smaller
Edit:

```js
embedFont = Math.floor(rainFont * 1.9);
```

### Increase or decrease vertical spread
Modify jitter range:

```js
const jitter = Math.floor(Math.random() * 13) - 6;
```

### Make rain denser or lighter
Adjust fade:

```js
ctx.fillStyle = "rgba(0,0,0,0.06)";
```

Higher value → faster fading → less dense.

---

## 🧪 Local Testing

You can test locally by running:

```
python3 -m http.server
```

Then visit:

```
http://localhost:8000/index.html
```

You'll see full animation (your e‑ink device will only snapshot frames every few seconds/minutes).

---

## 📌 Notes for e‑ink behavior on reTerminal

- E‑ink does **not** animate continuously  
- It only shows snapshots when SenseCraft refreshes  
- The animation runs in the background, so each refresh gives a new Matrix frame  
- Bold floating characters remain readable even with low-contrast shifts  

---

## 📝 License

This project is free for personal and commercial use. No attribution required.

---
