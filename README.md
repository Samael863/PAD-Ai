<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
  <title>PAD AI – iPad Version</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg: #07090d;
      --card: #0f121a;
      --border: #1f2533;
      --text: #e4e4e7;
      --muted: #71717a;
      --accent: #7c6bff;
      --green: #76e39a;
      --pink: #ff5a79;
      --yellow: #fbbf24;
    }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background: var(--bg);
      color: var(--text);
      height: 100dvh;
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }
    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 16px;
      border-bottom: 1px solid var(--border);
      background: rgba(7,9,13,0.9);
      backdrop-filter: blur(12px);
      flex-shrink: 0;
    }
    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .logo-icon {
      width: 32px;
      height: 32px;
      border-radius: 10px;
      background: linear-gradient(135deg, #7c6bff, #4f39ff);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 14px;
    }
    .logo-text { font-weight: 600; font-size: 15px; }
    .logo-sub { font-size: 11px; color: var(--muted); }
    .status {
      font-size: 11px;
      color: var(--muted);
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .dot {
      width: 7px;
      height: 7px;
      border-radius: 50%;
      background: var(--green);
    }
    .modes {
      display: flex;
      gap: 8px;
      padding: 10px 12px;
      overflow-x: auto;
      border-bottom: 1px solid var(--border);
      flex-shrink: 0;
      -webkit-overflow-scrolling: touch;
    }
    .mode-btn {
      flex-shrink: 0;
      padding: 8px 14px;
      border-radius: 12px;
      border: 1px solid var(--border);
      background: #0f131c;
      color: var(--muted);
      font-size: 13px;
      font-weight: 500;
      cursor: pointer;
    }
    .mode-btn.active {
      background: #151a27;
      color: white;
      border-color: #2a3142;
    }
    .mode-btn span { margin-right: 4px; }
    #chat {
      flex: 1;
      overflow-y: auto;
      padding: 16px 12px;
      -webkit-overflow-scrolling: touch;
    }
    .msg {
      max-width: 90%;
      margin-bottom: 14px;
      line-height: 1.5;
      font-size: 15px;
    }
    .msg.user {
      margin-left: auto;
      background: #151922;
      border: 1px solid var(--border);
      border-radius: 18px 18px 6px 18px;
      padding: 10px 14px;
    }
    .msg.ai {
      display: flex;
      gap: 10px;
    }
    .ai-avatar {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      background: linear-gradient(135deg, #7c6bff, #5a46ff);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 11px;
      font-weight: 700;
      flex-shrink: 0;
      margin-top: 2px;
    }
    .ai-bubble {
      background: var(--card);
      border: 1px solid #1c212e;
      border-radius: 6px 16px 16px 16px;
      padding: 10px 14px;
      white-space: pre-wrap;
      word-break: break-word;
    }
    .welcome {
      text-align: center;
      padding: 40px 20px;
      color: var(--muted);
    }
    .welcome h1 {
      font-size: 24px;
      color: white;
      margin-bottom: 8px;
      font-weight: 700;
    }
    .welcome p { font-size: 14px; line-height: 1.5; }
    .examples {
      display: grid;
      gap: 10px;
      margin-top: 24px;
      text-align: left;
    }
    .example {
      background: #0f131c;
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 12px 14px;
      cursor: pointer;
      font-size: 13px;
    }
    .example strong { display: block; color: white; margin-bottom: 2px; }
    .input-area {
      padding: 10px 12px 16px;
      border-top: 1px solid var(--border);
      background: rgba(7,9,13,0.95);
      flex-shrink: 0;
    }
    .input-row {
      display: flex;
      gap: 8px;
      align-items: flex-end;
    }
    textarea {
      flex: 1;
      background: #10141e;
      border: 1px solid #232a3a;
      border-radius: 16px;
      padding: 12px 14px;
      color: white;
      font-size: 15px;
      resize: none;
      max-height: 120px;
      min-height: 46px;
      outline: none;
      font-family: inherit;
    }
    textarea:focus { border-color: #2e374f; }
    .send-btn {
      width: 46px;
      height: 46px;
      border-radius: 14px;
      background: white;
      color: black;
      border: none;
      font-size: 18px;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      cursor: pointer;
    }
    .send-btn:disabled { opacity: 0.4; }
    .hint {
      font-size: 11px;
      color: var(--muted);
      margin-top: 6px;
      text-align: center;
    }
    pre {
      background: #0b0e14;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 10px;
      overflow-x: auto;
      font-size: 13px;
      margin: 8px 0;
    }
    code { font-family: ui-monospace, monospace; }
  </style>
</head>
<body>
  <header>
    <div class="logo">
      <div class="logo-icon">P</div>
      <div>
        <div class="logo-text">PAD AI</div>
        <div class="logo-sub">iPad Version</div>
      </div>
    </div>
    <div class="status">
      <div class="dot"></div>
      Demo • Offline
    </div>
  </header>

  <div class="modes" id="modes">
    <button class="mode-btn active" data-mode="assistant"><span>💬</span> Chat</button>
    <button class="mode-btn" data-mode="code"><span>💻</span> Code</button>
    <button class="mode-btn" data-mode="creative"><span>✨</span> Kreativ</button>
    <button class="mode-btn" data-mode="gaming"><span>🎮</span> Gaming</button>
  </div>

  <div id="chat">
    <div class="welcome" id="welcome">
      <h1>Was willst du bauen?</h1>
      <p>PAD AI – einfache Version fürs iPad.<br>Läuft komplett offline im Browser.</p>
      <div class="examples">
        <div class="example" data-prompt="Erklär mir kurz, wie man eine To-Do App mit HTML und JS baut.">
          <strong>💡 Projektidee</strong>
          Einfache To-Do App erklären
        </div>
        <div class="example" data-prompt="Schreib mir eine kurze Funktion in JavaScript, die eine Liste von Zahlen sortiert.">
          <strong>💻 Code-Hilfe</strong>
          JavaScript Sortierfunktion
        </div>
        <div class="example" data-prompt="Gib mir 3 ungewöhnliche SaaS-Ideen für Solo-Founder, die man schnell bauen kann.">
          <strong>✨ Kreativ</strong>
          3 SaaS-Ideen
        </div>
      </div>
    </div>
  </div>

  <div class="input-area">
    <div class="input-row">
      <textarea id="input" rows="1" placeholder="Frag PAD AI etwas..."></textarea>
      <button class="send-btn" id="send">↑</button>
    </div>
    <div class="hint">Enter = Senden • Shift+Enter = neue Zeile</div>
  </div>

  <script>
    const MODES = {
      assistant: {
        name: "Chat",
        system: "Du bist PAD AI, ein freundlicher, präziser deutscher KI-Assistent. Hilf praktisch und kurz. Antworte auf Deutsch, sei direkt, keine Floskeln."
      },
      code: {
        name: "Code",
        system: "Du bist PAD AI Code-Modus: Senior Dev. Erkläre kurz (max 3 Sätze), dann korrigierter Code in Markdown. Sei präzise."
      },
      creative: {
        name: "Kreativ",
        system: "Du bist PAD AI Kreativ-Modus: wild, originell. Liefere Ideen die man sofort bauen will. Deutsch."
      },
      gaming: {
        name: "Gaming",
        system: "Du bist PAD AI Gaming-Coach. Reagiere kurz, präzise und direkt auf Deutsch. Gib Tipps wie ein Pro-Coach."
      }
    };

    let currentMode = "assistant";
    let messages = [];
    const chatEl = document.getElementById("chat");
    const inputEl = document.getElementById("input");
    const sendBtn = document.getElementById("send");
    const welcomeEl = document.getElementById("welcome");

    // Mode switching
    document.querySelectorAll(".mode-btn").forEach(btn => {
      btn.addEventListener("click", () => {
        document.querySelectorAll(".mode-btn").forEach(b => b.classList.remove("active"));
        btn.classList.add("active");
        currentMode = btn.dataset.mode;
      });
    });

    // Example clicks
    document.querySelectorAll(".example").forEach(ex => {
      ex.addEventListener("click", () => {
        inputEl.value = ex.dataset.prompt;
        sendMessage();
      });
    });

    // Auto-resize textarea
    inputEl.addEventListener("input", () => {
      inputEl.style.height = "auto";
      inputEl.style.height = Math.min(inputEl.scrollHeight, 120) + "px";
    });

    // Send on Enter
    inputEl.addEventListener("keydown", e => {
      if (e.key === "Enter" && !e.shiftKey) {
        e.preventDefault();
        sendMessage();
      }
    });

    sendBtn.addEventListener("click", sendMessage);

    function addMessage(role, text) {
      if (welcomeEl) welcomeEl.style.display = "none";

      const div = document.createElement("div");
      div.className = `msg ${role}`;

      if (role === "user") {
        div.textContent = text;
      } else {
        div.innerHTML = `
          <div class="ai-avatar">AI</div>
          <div class="ai-bubble">${formatText(text)}</div>
        `;
      }
      chatEl.appendChild(div);
      chatEl.scrollTop = chatEl.scrollHeight;
    }

    function formatText(text) {
      // Simple markdown-like formatting
      return text
        .replace(/```([\s\S]*?)```/g, '<pre><code>$1</code></pre>')
        .replace(/`([^`]+)`/g, '<code>$1</code>')
        .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
        .replace(/\n/g, '<br>');
    }

    function generateReply(userText) {
      const lower = userText.toLowerCase();
      const mode = MODES[currentMode];

      // Simple rule-based demo replies (works fully offline)
      if (currentMode === "code" || lower.includes("code") || lower.includes("funktion") || lower.includes("javascript") || lower.includes("html")) {
        return `Hier ist eine saubere Lösung:

\`\`\`javascript
function sortNumbers(arr) {
  return [...arr].sort((a, b) => a - b);
}

// Beispiel
const zahlen = [5, 2, 9, 1, 7];
console.log(sortNumbers(zahlen)); // [1, 2, 5, 7, 9]
\`\`\`

Die Funktion erstellt eine Kopie der Liste und sortiert sie aufsteigend. Willst du eine Version mit absteigender Sortierung oder mit Objekten?`;
      }

      if (currentMode === "creative" || lower.includes("idee") || lower.includes("saas")) {
        return `**3 schnell baubare Ideen:**

1. **Fokus-Timer mit Poster**
   25-Minuten-Timer + am Ende generiert es ein kleines Poster deines Fokus-Tags (Canvas).

2. **Unsichtbares Tagebuch**
   Nur im Browser, verschlüsselt mit WebCrypto, kein Server.

3. **Snippet-Board**
   Speichere Code-Snippets lokal und teile sie per Link (IndexedDB + Share API).

Welche soll ich dir als erstes als fertigen HTML-Code ausgeben?`;
      }

      if (currentMode === "gaming" || lower.includes("spiel") || lower.includes("game")) {
        return `Klar! Hier ein paar schnelle Tipps:

- Halte immer High Ground wenn möglich
- Sound ist wichtiger als du denkst – Kopfhörer helfen
- Übe Crosshair-Placement in Aim-Trainern
- Nach jedem Tod kurz analysieren: Was war der Fehler?

Willst du ein kleines Canvas-Spiel als HTML-Code, das du direkt testen kannst?`;
      }

      // Default assistant reply
      return `Hey! Ich bin PAD AI (iPad-Version).

Deine Frage: „${userText.slice(0, 80)}${userText.length > 80 ? '…' : ''}“

Ich kann dir helfen bei:
• Code schreiben & debuggen
• Projekt-Ideen finden
• Kurze Erklärungen
• Gaming-Tipps

Sag mir einfach genauer, was du brauchst – ich antworte direkt und ohne Umschweife.`;
    }

    async function sendMessage() {
      const text = inputEl.value.trim();
      if (!text) return;

      inputEl.value = "";
      inputEl.style.height = "auto";
      sendBtn.disabled = true;

      addMessage("user", text);
      messages.push({ role: "user", content: text });

      // Simulate thinking
      const thinking = document.createElement("div");
      thinking.className = "msg ai";
      thinking.innerHTML = `
        <div class="ai-avatar">AI</div>
        <div class="ai-bubble" style="opacity:0.6">Denkt …</div>
      `;
      chatEl.appendChild(thinking);
      chatEl.scrollTop = chatEl.scrollHeight;

      await new Promise(r => setTimeout(r, 600 + Math.random() * 800));

      thinking.remove();
      const reply = generateReply(text);
      addMessage("ai", reply);
      messages.push({ role: "assistant", content: reply });

      sendBtn.disabled = false;
      inputEl.focus();
    }

    // Save/load simple history
    try {
      const saved = localStorage.getItem("pad_ipad_messages");
      if (saved) {
        const parsed = JSON.parse(saved);
        if (parsed.length) {
          welcomeEl.style.display = "none";
          parsed.forEach(m => addMessage(m.role === "user" ? "user" : "ai", m.content));
          messages = parsed;
        }
      }
    } catch(e) {}

    window.addEventListener("beforeunload", () => {
      try {
        localStorage.setItem("pad_ipad_messages", JSON.stringify(messages.slice(-40)));
      } catch(e) {}
    });
  </script>
</body>
</html>
