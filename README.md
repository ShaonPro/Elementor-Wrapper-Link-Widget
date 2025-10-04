# Elementor Global Wrapper Link (All Widgets)

Add a **Wrapper Link** control to **all Elementor widgets** — no plugin needed.  
Just drop this PHP snippet into your theme’s `functions.php` or a custom snippets manager.

---

## ✨ Features

- Adds a **"Wrapper Link"** option to **every Elementor widget**.
- Two modes:
  - **Safe (JS)** – Keeps inner links & buttons clickable.
  - **Overlay (CSS)** – Full clickable cover (blocks inner links).
- Supports:
  - External links (`target="_blank"`)
  - `nofollow` and `noopener` attributes
  - Keyboard accessibility (focus + Enter key)
- Lightweight — no external JS/CSS files (inline only).
- Works only if **Elementor** is active (safe check included).

---

## 🧩 How to Use

1. Copy the full code from `elementor-wrapper-link.php`.
2. Paste it into:
   - Your theme’s `functions.php`, **or**
   - A code snippets plugin (like *Code Snippets*).
3. Save and reload your Elementor editor.
4. In any widget:
   - Go to **Advanced → Wrapper Link**
   - Enable it
   - Add your link URL
   - Choose your preferred mode (JS or Overlay)
5. Update your page — the entire widget is now clickable.

---

## ⚙️ Mode Comparison

| Mode | Description | Inner Links |
|------|--------------|--------------|
| **Safe (JS)** | Adds JS click handler (recommended) | ✅ Clickable |
| **Overlay (CSS)** | Adds full overlay anchor | 🚫 Blocked |

---

## 🧠 Tips

- Use **Safe (JS)** for cards, boxes, and sections containing buttons or links.  
- Use **Overlay (CSS)** for simple widgets (like images or plain content).  
- Works in both Elementor **Free** and **Pro**.

---

## 📋 Requirements

- WordPress 5.0 or higher  
- Elementor 3.0 or higher

---

## 🧾 License

Released under the **GPL-2.0-or-later** license.  
Free to use, modify, and share — attribution appreciated.

---

**Made with ❤️ by [ShaonPro](https://github.com/ShaonPro)**  
*“Make any Elementor widget clickable — the smart way.”*
