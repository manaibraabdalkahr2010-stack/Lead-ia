<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lead AI Game Engine</title>
    <style>
        /* تصميم سيبراني مخصص لمحرك الألعاب */
        :root {
            --bg-main: #05060f;
            --bg-sidebar: #0a0c1a;
            --neon-pink: #ff007f;
            --neon-cyan: #00f0ff;
            --neon-green: #39ff14;
            --text-color: #e2e8f0;
        }

        body {
            margin: 0; padding: 0;
            background-color: var(--bg-main);
            color: var(--text-color);
            font-family: 'Segoe UI', Roboto, sans-serif;
            display: flex; height: 100vh; overflow: hidden;
        }

        /* شاشة تسجيل الدخول */
        #login-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(5, 6, 15, 0.98); z-index: 1000;
            display: flex; justify-content: center; align-items: center;
        }
        .login-box {
            background: var(--bg-sidebar); padding: 40px; border-radius: 20px;
            border: 2px solid var(--neon-cyan); text-align: center; width: 350px;
            box-shadow: 0 0 25px rgba(0, 240, 255, 0.4);
        }
        .login-box h2 { color: var(--neon-cyan); margin-bottom: 25px; text-shadow: 0 0 10px rgba(0, 240, 255, 0.5); }
        .login-box input {
            width: 90%; padding: 14px; margin-bottom: 20px;
            background: #111430; border: 1px solid #22295c;
            color: white; border-radius: 10px; text-align: center; outline: none; font-size: 16px;
        }
        .login-box button {
            background: linear-gradient(45deg, var(--neon-cyan), #0072ff); color: white; border: none;
            padding: 12px 30px; border-radius: 10px; cursor: pointer; font-weight: bold; font-size: 16px;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.4); transition: 0.3s;
        }
        .login-box button:hover { transform: scale(1.05); box-shadow: 0 0 25px var(--neon-cyan); }

        /* شريط الحالة العلوي */
        .top-status-bar {
            position: absolute; top: 0; left: 0; width: 100%;
            background: linear-gradient(90deg, #090b1e, #031411);
            border-bottom: 2px solid #112552; padding: 12px 20px;
            display: flex; justify-content: space-between; align-items: center;
            box-sizing: border-box; z-index: 10;
        }
        .dev-name { color: #ffffff; font-size: 14px; font-weight: bold; }
        .upgrade-msg { color: var(--neon-cyan); font-size: 14px; font-weight: bold; text-shadow: 0 0 8px rgba(0, 240, 255, 0.4); }

        /* القائمة الجانبية */
        .sidebar {
            width: 30%; background-color: var(--bg-sidebar);
            padding: 85px 20px 20px 20px; display: flex; flex-direction: column; gap: 18px;
            box-sizing: border-box; border-inline-end: 2px solid #131735;
        }
        .logo-area { text-align: center; margin-bottom: 10px; }
        .logo-icon { font-size: 45px; color: var(--neon-pink); font-weight: bold; text-shadow: 0 0 15px var(--neon-pink); }
        .logo-title { color: #ffffff; font-size: 24px; font-weight: bold; margin-top: 5px; }
        .logo-sub { color: #8a94ad; font-size: 14px; margin-top: 4px; }
        .owner-highlight { color: var(--neon-pink); text-shadow: 0 0 5px var(--neon-pink); }

        /* الأزرار */
        .room-btn {
            background: linear-gradient(135deg, #11102b, #24072b);
            border: 1px solid var(--neon-cyan); border-radius: 12px; padding: 15px;
            color: #ffffff; font-size: 15px; font-weight: bold; cursor: pointer;
            display: flex; justify-content: space-between; align-items: center;
            transition: 0.3s;
        }
        .room-btn:hover { box-shadow: 0 0 15px var(--neon-cyan); }
        
        .room-form-input {
            background: linear-gradient(135deg, #031c24, #08121a);
            border: 1px solid var(--neon-pink); border-radius: 12px; padding: 12px;
            display: flex; align-items: center; gap: 8px;
        }
        .room-form-input input { background: transparent; border: none; color: white; outline: none; flex: 1; font-size: 14px; }
        .room-form-input button { background: transparent; border: none; cursor: pointer; font-size: 18px; color: var(--neon-pink); }

        /* سجل محرك الألعاب */
        .history-section { margin-top: 15px; flex: 1; display: flex; flex-direction: column; overflow: hidden; }
        .history-title { color: #6b7c96; font-size: 13px; font-weight: bold; margin-bottom: 10px; text-transform: uppercase; }
        .history-list { overflow-y: auto; flex: 1; display: flex; flex-direction: column; gap: 8px; }
        .history-item {
            background: #101330; padding: 12px; border-radius: 10px; font-size: 14px; cursor: pointer;
            border-right: 4px solid var(--neon-pink); transition: 0.2s; white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
        }
        .history-item:hover, .history-item.active { background: #181d47; border-right-color: var(--neon-cyan); }

        /* منطقة الشات الرئيسية */
        .main-content {
            width: 70%; display: flex; flex-direction: column;
            padding: 80px 25px 100px 25px; box-sizing: border-box; position: relative; background: #03040a;
        }
        .welcome-screen {
            display: flex; flex-direction: column; align-items: center; justify-content: center; flex: 1; text-align: center;
        }
        .main-title { color: var(--neon-cyan); font-size: 36px; font-weight: bold; margin: 0; text-shadow: 0 0 15px var(--neon-cyan); }
        .crown-icon { font-size: 50px; margin: 15px 0; }
        .main-desc { color: #8a94ad; font-size: 16px; max-width: 450px; line-height: 1.6; }

        /* صندوق محادثات السيرفر */
        .chat-box { flex: 1; overflow-y: auto; display: none; flex-direction: column; gap: 15px; padding: 10px 5px; }
        .msg { padding: 14px 18px; border-radius: 14px; max-width: 80%; line-height: 1.6; font-size: 15px; word-wrap: break-word; }
        .user-msg { background-color: #151730; align-self: flex-start; border-bottom-left-radius: 2px; border-left: 3px solid var(--neon-cyan); }
        .ai-msg { background-color: #041c17; border: 1px solid #0b4034; align-self: flex-end; border-bottom-right-radius: 2px; border-right: 3px solid var(--neon-green); }
        
        pre {
            background: #000000; color: #39ff14; padding: 15px; border-radius: 8px;
            overflow-x: auto; font-family: 'Courier New', Courier, monospace; direction: ltr; text-align: left;
            border: 1px solid #111; margin: 10px 0; box-shadow: inset 0 0 10px rgba(57, 255, 20, 0.2);
        }

        /* شريط الإدخال */
        .bottom-search-area { position: absolute; bottom: 20px; left: 5%; width: 90%; box-sizing: border-box; }
        .search-form {
            width: 100%; background: linear-gradient(90deg, #0a0d24, #031411);
            border: 2px solid var(--neon-pink); border-radius: 35px; padding: 14px 25px;
            display: flex; align-items: center; box-sizing: border-box;
            box-shadow: 0 0 20px rgba(255, 0, 127, 0.2);
        }
        .search-form:focus-within { border-color: var(--neon-cyan); box-shadow: 0 0 25px rgba(0, 240, 255, 0.4); }
        .search-form input { width: 100%; background: transparent; border: none; color: #ffffff; font-size: 16px; outline: none; }
        .search-btn { background: transparent; border: none; cursor: pointer; font-size: 20px; color: var(--neon-pink); }
        .search-btn:hover { color: var(--neon-cyan); }
    </style>
</head>
<body>

    <div id="login-overlay">
        <div class="login-box">
            <h2>Lead AI Game Engine</h2>
            <input type="text" id="username-input" placeholder="اسم مطور الألعاب..." value="صالح جابر" required>
            <button onclick="loginUser()">تشغيل المحرك 🎮</button>
        </div>
    </div>

    <div class="top-status-bar">
        <div class="dev-name" id="user-display">النظام: غير متصل</div>
        <div class="upgrade-msg">⚡ Lead AI Game Engine Dedicated Mode</div>
    </div>

    <div class="sidebar">
        <div class="logo-area">
            <div class="logo-icon">🕹️</div>
            <div class="logo-title">Lead AI Engine</div>
            <div class="logo-sub">المالك المطور: <span class="owner-highlight">صالح جابر</span></div>
        </div>

        <div class="room-btn" onclick="createNewChat()">
            <span>إنشاء مشروع محادثة ألعاب</span>
            <span>➕</span>
        </div>

        <div class="room-form-input">
            <input type="text" id="room-code-input" placeholder="رمز الغرفة المشفرة للألعاب...">
            <button onclick="joinRoomByCode()">🔑</button>
        </div>

        <div class="history-section">
            <div class="history-title">جلسات أكواد الألعاب الحالية</div>
            <div class="history-list" id="history-list"></div>
        </div>
    </div>

    <div class="main-content">
        <div class="welcome-screen" id="welcome-screen">
            <div class="main-title">Lead AI Game Engine</div>
            <div class="crown-icon">🚀</div>
            <div class="main-desc">هذه النسخة مخصصة كلياً للألعاب وتطوير الأكواد بسرعة فائقة. اطلب أي كود لعبة وسيقوم المحرك بتوليده لك متصل بالكامل وبدون تشتيت!</div>
        </div>

        <div class="chat-box" id="chat-box"></div>

        <div class="bottom-search-area">
            <div class="search-form">
                <input type="text" id="query-input" placeholder="✨ اطلب كود لعبة أو ميزة برمجية للمحرك سريع سريع..." onkeypress="checkEnter(event)">
                <button class="search-btn" onclick="processQuestion()">🔍</button>
            </div>
        </div>
    </div>

    <script>
        let conversations = {};
        let currentChatId = null;

        function loginUser() {
            const name = document.getElementById('username-input').value.trim();
            if(name !== "") {
                document.getElementById('user-display').innerText = "المطور: " + name;
                document.getElementById('login-overlay').style.display = 'none';
                createNewChat();
            }
        }

        function createNewChat() {
            const chatId = 'game_chat_' + Date.now();
            const roomCode = Math.floor(100000 + Math.random() * 900000);
            
            conversations[chatId] = {
                title: "مشروع لعبة #" + Object.keys(conversations).length,
                code: roomCode,
                messages: []
            };

            currentChatId = chatId;
            renderHistoryList();
            loadChat(chatId);
        }

        function renderHistoryList() {
            const listContainer = document.getElementById('history-list');
            listContainer.innerHTML = "";

            Object.keys(conversations).forEach(chatId => {
                const item = document.createElement('div');
                item.className = 'history-item' + (chatId === currentChatId ? ' active' : '');
                item.innerText = conversations[chatId].title + " (" + conversations[chatId].code + ")";
                item.onclick = () => loadChat(chatId);
                listContainer.appendChild(item);
            });
        }

        function loadChat(chatId) {
            currentChatId = chatId;
            renderHistoryList();

            const chatBox = document.getElementById('chat-box');
            const welcomeScreen = document.getElementById('welcome-screen');
            const currentChat = conversations[chatId];

            if (currentChat.messages.length === 0) {
                chatBox.style.display = 'none';
                welcomeScreen.style.display = 'flex';
                welcomeScreen.querySelector('.main-desc').innerText = "المحرك جاهز في هذه الغرفة المشفرة بالرمز: " + currentChat.code + ". اطلب كود اللعبة الآن.";
            } else {
                welcomeScreen.style.display = 'none';
                chatBox.style.display = 'flex';
                chatBox.innerHTML = "";

                currentChat.messages.forEach(msg => {
                    const msgDiv = document.createElement('div');
                    msgDiv.className = 'msg ' + (msg.sender === 'user' ? 'user-msg' : 'ai-msg');
                    msgDiv.innerHTML = msg.text;
                    chatBox.appendChild(msgDiv);
                });
                chatBox.scrollTop = chatBox.scrollHeight;
            }
        }

        function processQuestion() {
            const inputField = document.getElementById('query-input');
            const query = inputField.value.trim();
            if (query === "" || !currentChatId) return;

            const currentChat = conversations[currentChatId];

            if (currentChat.messages.length === 0) {
                currentChat.title = query.length > 18 ? query.substring(0, 18) + "..." : query;
                renderHistoryList();
            }

            currentChat.messages.push({ sender: 'user', text: query });
            loadChat(currentChatId);
            inputField.value = "";

            setTimeout(() => {
                const aiResponse = generateGameEngineResponse(query);
                currentChat.messages.push({ sender: 'ai', text: aiResponse });
                loadChat(currentChatId);
            }, 400);
        }

        // محرك الردود المخصص فقط للألعاب وسريع سريع
        function generateGameEngineResponse(userQuery) {
            const q = userQuery.toLowerCase();

            if (q.includes("ماين كرافت") || q.includes("minecraft") || q.includes("كود") || q.includes("لعبة")) {
                return "مرحباً مطورنا صالح! محرك الألعاب تولى الطلب، إليك كود اللعبة المطلوب فوراً كامل وجاهز للتشغيل: <br><pre>&lt;!DOCTYPE html&gt;\n&lt;html&gt;\n&lt;head&gt;\n  &lt;title&gt;Lead AI Game Output&lt;/title&gt;\n&lt;/head&gt;\n&lt;body style='background:#050510; color:#fff; text-align:center; font-family:sans-serif;'&gt;\n  &lt;h1&gt;Lead AI Game Engine Pixel Clicker&lt;/h1&gt;\n  &lt;p&gt;النقاط المجمعة: &lt;span id='score'&gt;0&lt;/span&gt;&lt;/p&gt;\n  &lt;div id='block' style='width:120px; height:120px; background:#ff007f; margin:auto; cursor:pointer; box-shadow:0 0 20px #ff007f;' onclick='clickGame()'&gt;&lt;/div&gt;\n  &lt;script&gt;\n    let score = 0;\n    function clickGame() {\n      score++;\n      document.getElementById(\"score\").innerText = score;\n    }\n  &lt;/script&gt;\n&lt;/body&gt;\n&lt;/html&gt;</pre>";
            }

            return "مستشعر محرك الألعاب نشط. يرجى تحديد كود اللعبة أو الميكانيكس البرمجي (Game Mechanics) الذي تريد توليده سريع سريع لتنفيذه في هذه الجلسة.";
        }

        function joinRoomByCode() {
            const codeInput = document.getElementById('room-code-input');
            const code = codeInput.value.trim();
            if (code === "") return;

            let foundChatId = null;
            Object.keys(conversations).forEach(id => {
                if (conversations[id].code.toString() === code) { foundChatId = id; }
            });

            if (foundChatId) {
                loadChat(foundChatId);
            } else {
                const chatId = 'game_chat_' + Date.now();
                conversations[chatId] = {
                    title: "غرفة محرك منضمّة " + code,
                    code: code,
                    messages: [{ sender: 'ai', text: "تم الدخول لغرفة تطوير ألعاب مخصصة بالرمز " + code }]
                };
                loadChat(chatId);
            }
            codeInput.value = "";
        }

        function checkEnter(event) {
            if (event.key === 'Enter') { processQuestion(); }
        }
    </script>
</body>
</html>

