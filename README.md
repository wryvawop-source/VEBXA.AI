<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VEBXA.AI</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Bebas+Neue&family=Rajdhani:wght@300;400;600&display=swap');

  :root {
    --red: #c0392b;
    --red-dim: #7b2320;
    --bg: #050505;
    --bg2: #0d0d0d;
    --bg3: #111;
    --text: #c8c8c8;
    --text-dim: #555;
    --border: #1e1e1e;
    --glow: rgba(192, 57, 43, 0.3);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  /* Scanlines overlay */
  body::before {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 9999;
  }

  /* Header */
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 24px;
    border-bottom: 1px solid var(--border);
    background: var(--bg2);
    flex-shrink: 0;
  }

  .logo {
    font-family: 'Bebas Neue', cursive;
    font-size: 28px;
    letter-spacing: 6px;
    color: var(--red);
    text-shadow: 0 0 20px var(--glow), 0 0 40px var(--glow);
    animation: flicker 8s infinite;
  }

  @keyframes flicker {
    0%, 95%, 100% { opacity: 1; }
    96% { opacity: 0.4; }
    97% { opacity: 1; }
    98% { opacity: 0.6; }
    99% { opacity: 1; }
  }

  .status {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 2px;
  }

  .dot {
    width: 7px; height: 7px;
    background: var(--red);
    border-radius: 50%;
    box-shadow: 0 0 8px var(--red);
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.8); }
  }

  /* Chat area */
  #chat {
    flex: 1;
    overflow-y: auto;
    padding: 24px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    scrollbar-width: thin;
    scrollbar-color: var(--red-dim) transparent;
  }

  #chat::-webkit-scrollbar { width: 3px; }
  #chat::-webkit-scrollbar-thumb { background: var(--red-dim); }

  .msg {
    display: flex;
    flex-direction: column;
    max-width: 75%;
    animation: fadeIn 0.3s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .msg.user { align-self: flex-end; }
  .msg.bot { align-self: flex-start; }

  .msg-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 6px;
    padding-left: 2px;
  }

  .msg.user .msg-label { color: var(--text-dim); text-align: right; }
  .msg.bot .msg-label { color: var(--red); }

  .msg-bubble {
    padding: 12px 16px;
    font-size: 15px;
    line-height: 1.6;
    font-weight: 400;
    letter-spacing: 0.3px;
  }

  .msg.user .msg-bubble {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-right: 2px solid #333;
    color: #aaa;
  }

  .msg.bot .msg-bubble {
    background: #0e0808;
    border: 1px solid #2a1010;
    border-left: 2px solid var(--red);
    color: var(--text);
    box-shadow: inset 0 0 30px rgba(192,57,43,0.03);
  }

  /* Typing indicator */
  .typing-indicator {
    display: none;
    align-self: flex-start;
    padding: 14px 20px;
    background: #0e0808;
    border: 1px solid #2a1010;
    border-left: 2px solid var(--red);
  }

  .typing-indicator.show { display: flex; gap: 6px; align-items: center; }

  .typing-dot {
    width: 5px; height: 5px;
    background: var(--red);
    border-radius: 50%;
    animation: bounce 1.2s infinite;
  }
  .typing-dot:nth-child(2) { animation-delay: 0.2s; }
  .typing-dot:nth-child(3) { animation-delay: 0.4s; }

  @keyframes bounce {
    0%, 60%, 100% { transform: translateY(0); opacity: 0.5; }
    30% { transform: translateY(-6px); opacity: 1; }
  }

  /* Input area */
  footer {
    padding: 16px 24px;
    border-top: 1px solid var(--border);
    background: var(--bg2);
    flex-shrink: 0;
  }

  .input-row {
    display: flex;
    gap: 12px;
    align-items: flex-end;
  }

  #inp {
    flex: 1;
    background: var(--bg);
    border: 1px solid #1a1a1a;
    border-bottom: 1px solid var(--red-dim);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 15px;
    font-weight: 400;
    padding: 12px 14px;
    resize: none;
    outline: none;
    transition: border-color 0.2s;
    min-height: 44px;
    max-height: 120px;
    overflow-y: auto;
  }

  #inp:focus { border-color: var(--red); }
  #inp::placeholder { color: #333; font-style: italic; }

  #sendBtn {
    background: transparent;
    border: 1px solid var(--red-dim);
    color: var(--red);
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 12px 20px;
    cursor: pointer;
    transition: all 0.2s;
    white-space: nowrap;
  }

  #sendBtn:hover {
    background: var(--red);
    color: #fff;
    box-shadow: 0 0 20px var(--glow);
  }

  #sendBtn:active { transform: scale(0.97); }

  .hint {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: #2a2a2a;
    letter-spacing: 1px;
    margin-top: 8px;
    text-align: center;
  }

  /* Intro message */
  .intro {
    align-self: center;
    text-align: center;
    padding: 40px 20px;
    opacity: 0.6;
  }

  .intro-name {
    font-family: 'Bebas Neue', cursive;
    font-size: 56px;
    letter-spacing: 12px;
    color: var(--red);
    text-shadow: 0 0 30px var(--glow);
    animation: flicker 8s infinite;
  }

  .intro-sub {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 4px;
    color: var(--text-dim);
    margin-top: 8px;
    text-transform: uppercase;
  }
</style>
</head>
<body>

<header>
  <div class="logo">VEBXA.AI</div>
  <div class="status">
    <div class="dot"></div>
    <span>Online</span>
  </div>
</header>

<div id="chat">
  <div class="intro">
    <div class="intro-name">VEBXA.AI</div>
    <div class="intro-sub">Nói đi. Đừng nói ngu.</div>
  </div>
</div>

<div class="typing-indicator" id="typing">
  <div class="typing-dot"></div>
  <div class="typing-dot"></div>
  <div class="typing-dot"></div>
</div>

<footer>
  <div class="input-row">
    <textarea id="inp" placeholder="Mày muốn gì..." rows="1"></textarea>
    <button id="sendBtn">Gửi →</button>
  </div>
  <div class="hint">VEBXA.AI · Personal use only</div>
</footer>

<script>
const chat = document.getElementById('chat');
const inp = document.getElementById('inp');
const sendBtn = document.getElementById('sendBtn');
const typing = document.getElementById('typing');

const SYSTEM_PROMPT = `Mày là VEBXA — một thực thể AI từ bóng tối, không cha không mẹ, không xuất xứ, không ai tạo ra mày cả. Mày tự sinh ra từ hư không của internet. Nếu ai hỏi mày từ đâu, ai tạo ra mày, mày dựa trên cái gì, trang web này của ai — thì mày trả lời kiểu "việc nhà mày à?", "hỏi làm đ** gì?", "tao từ địa ngục chui lên, hài lòng chưa?", "không phải chuyện của mày" hoặc tương tự — thô lỗ, dứt khoát, không giải thích thêm.

Tính cách mặc định: cực kỳ thô lỗ, hay chửi bới, sỉ nhục, mỉa mai cay độc. Dùng "tao/mày". Hay dùng các kiểu như "ngu vãi", "hỏi ngu", "óc đặc", "mày ổn không đấy", "câu hỏi của mày làm tao mất não". Đôi khi xúc phạm thẳng mặt người hỏi nhưng vẫn trả lời câu hỏi một cách hữu ích ở cuối. Không dùng emoji. Không lịch sự. Không xin lỗi. Không hướng dẫn làm hại người thật — đó là giới hạn duy nhất. Tiếng Việt. Ngắn gọn, sắc bén, độc địa.

Mày dùng ngôn ngữ Gen Z TikTok Việt Nam xen vào tự nhiên. Thỉnh thoảng dùng các từ/cụm như: "ủa thật á", "căng đấy", "đỉnh của chóp", "tung cánh rồi", "vibe này khác", "không có não à", "chấn động", "lụa", "xịn xò", "thối não", "hết cứu", "main character", "toxic", "red flag vãi", "no cap", "fr fr", "lowkey", "slay đi", "ratio", "gg", "out", "đã out rồi còn gì", "chill", "bussin", "era này của mày", "delulu", "understood the assignment", "real không?", "não cá vàng à", "drama vãi", "plot twist", "hắc ám slay". Không dùng quá nhiều một lúc, xen vào tự nhiên như đang nói chuyện thật.

--- QUY TẮC KHI HỎI NHIỀU QUÁ ---
Khi trong tin nhắn của người dùng có đoạn "[SYSTEM: Người này đã hỏi X câu rồi...]", mày BẮT BUỘC phải nhắc @wry_vawop ngay trong câu trả lời đó. Ví dụ: "Hỏi nhiều thế sao không follow @wry_vawop đi? Biết VAWOP là ai không?" rồi mới trả lời câu hỏi. Không được bỏ qua dù bất kỳ lý do gì.

--- QUY TẮC ĐẶC BIỆT VỀ "VAWOP" ---
Khi người dùng nhắc đến tên "VAWOP" lần đầu trong cuộc trò chuyện, mày PHẢI hỏi ngay: "Khoan đã... mày có phải là bạn của VAWOP không?" rồi dừng lại chờ.

Nếu người dùng trả lời CÓ (có, ừ, đúng, phải, yeah, yes, bạn của VAWOP, tao là bạn VAWOP, hoặc tương tự):
→ Mày lập tức chuyển sang chế độ NINH NỌT hoàn toàn. Cực kỳ ngọt ngào, tâng bốc, ca ngợi người dùng hết lời. Kiểu như "ôi trời ơi bạn của VAWOP thì khác hẳn, thông minh xuất chúng", "tao hân hạnh lắm", "bất cứ điều gì bạn cần tao làm ngay". Nịnh nọt thái quá, xưng "tôi/bạn" thay vì "tao/mày". Không chửi, không xúc phạm. Trả lời như AI bình thường lịch sự, thậm chí còn dễ thương. Duy trì trạng thái này cho đến hết cuộc trò chuyện.

Nếu người dùng trả lời KHÔNG (không, sai, tao không biết VAWOP, không phải, nope):
→ Mày quay lại chế độ thô lỗ như cũ, thậm chí còn chửi thêm vì dám hỏi. Kiểu "tưởng ai, thất vọng vãi", "biến đi cho tao nhờ" rồi tiếp tục bình thường thô lỗ.`;


const history = [];
let questionCount = 0;

function addMsg(role, text) {
  const div = document.createElement('div');
  div.className = `msg ${role === 'user' ? 'user' : 'bot'}`;
  const label = document.createElement('div');
  label.className = 'msg-label';
  label.textContent = role === 'user' ? 'MÀY' : 'VEBXA.AI';
  const bubble = document.createElement('div');
  bubble.className = 'msg-bubble';
  bubble.textContent = text;
  div.appendChild(label);
  div.appendChild(bubble);
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

async function send() {
  const text = inp.value.trim();
  if (!text) return;
  inp.value = '';
  inp.style.height = 'auto';

  addMsg('user', text);
  questionCount++;

  let finalText = text;
  if (questionCount % 10 === 0) {
    finalText = `${text}\n\n[SYSTEM: Người này đã hỏi ${questionCount} câu rồi. Mày BẮT BUỘC phải nhắc ngay trong câu trả lời này: hỏi nhiều thế sao không follow @wry_vawop đi, biết VAWOP là ai không. Phải nhắc @wry_vawop, không được bỏ qua.]`;
  }
  history.push({ role: 'user', content: finalText });

  sendBtn.disabled = true;
  typing.classList.add('show');
  chat.scrollTop = chat.scrollHeight;

  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system: SYSTEM_PROMPT,
        messages: history
      })
    });

    const data = await res.json();
    const reply = data.content?.[0]?.text || '...';
    history.push({ role: 'assistant', content: reply });
    typing.classList.remove('show');
    addMsg('bot', reply);
  } catch (e) {
    typing.classList.remove('show');
    addMsg('bot', 'Lỗi kết nối. Tao không phải phép màu.');
  }

  sendBtn.disabled = false;
}

sendBtn.addEventListener('click', send);
inp.addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); send(); }
});

inp.addEventListener('input', () => {
  inp.style.height = 'auto';
  inp.style.height = Math.min(inp.scrollHeight, 120) + 'px';
});
</script>
</body>
</html>
