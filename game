<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>梅露露的小世界</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: "Microsoft YaHei", sans-serif;
            overflow: hidden;
            height: 100vh;
            touch-action: none;
        }

        /* 场景背景 */
        .scene {
            width: 100%;
            height: 100vh;
            position: relative;
            transition: background 2s;
        }
        .scene.morning { background: linear-gradient(180deg, #87CEEB 0%, #FFF8DC 50%, #90EE90 100%); }
        .scene.afternoon { background: linear-gradient(180deg, #4FC3F7 0%, #81D4FA 50%, #A5D6A7 100%); }
        .scene.evening { background: linear-gradient(180deg, #FF8A65 0%, #FFB74D 40%, #455A64 100%); }
        .scene.night { background: linear-gradient(180deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%); }

        /* 顶部状态栏 */
        .top-bar {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            padding: 10px 15px;
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 100;
            color: white;
            font-size: 12px;
        }
        .top-bar .time-display { font-size: 14px; font-weight: bold; }
        .top-bar .coins { color: #ffd700; }

        /* 状态面板 */
        .stats-panel {
            position: fixed;
            top: 45px;
            left: 10px;
            right: 10px;
            background: rgba(255,255,255,0.9);
            border-radius: 12px;
            padding: 10px 15px;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            z-index: 90;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .stat-item {
            flex: 1;
            min-width: 45%;
        }
        .stat-item .label { font-size: 11px; color: #666; }
        .stat-bar {
            height: 8px;
            background: #eee;
            border-radius: 4px;
            overflow: hidden;
            margin-top: 3px;
        }
        .stat-bar-fill {
            height: 100%;
            border-radius: 4px;
            transition: width 0.5s;
        }
        .hunger .stat-bar-fill { background: linear-gradient(90deg, #ff6b6b, #ee5a24); }
        .mood .stat-bar-fill { background: linear-gradient(90deg, #ffd93d, #f39c12); }
        .energy .stat-bar-fill { background: linear-gradient(90deg, #6bcb77, #27ae60); }
        .love .stat-bar-fill { background: linear-gradient(90deg, #fd79a8, #e84393); }

        /* 梅露露角色 */
        .character {
            position: absolute;
            bottom: 180px;
            left: 50%;
            transform: translateX(-50%);
            text-align: center;
        }
        .character .sprite {
            font-size: 80px;
            animation: float 3s ease-in-out infinite;
        }
        .character .name-tag {
            background: rgba(255,255,255,0.8);
            padding: 3px 10px;
            border-radius: 10px;
            font-size: 12px;
            margin-top: 5px;
            display: inline-block;
        }
        .character .emotion {
            position: absolute;
            top: -20px;
            right: -10px;
            font-size: 24px;
            animation: pop 0.5s ease;
        }

        @keyframes float {
            0%, 100% { transform: translateX(-50%) translateY(0); }
            50% { transform: translateX(-50%) translateY(-10px); }
        }
        @keyframes pop {
            0% { transform: scale(0); }
            50% { transform: scale(1.3); }
            100% { transform: scale(1); }
        }
        @keyframes shake {
            0%, 100% { transform: translateX(-50%) rotate(0); }
            25% { transform: translateX(-50%) rotate(-5deg); }
            75% { transform: translateX(-50%) rotate(5deg); }
        }
        .character.eating .sprite { animation: shake 0.3s infinite; }
        .character.sleeping .sprite { animation: none; opacity: 0.7; }

        /* 对话气泡 */
        .chat-bubble {
            position: absolute;
            bottom: 290px;
            left: 50%;
            transform: translateX(-50%);
            background: white;
            border: 2px solid #ff69b4;
            border-radius: 15px;
            padding: 10px 15px;
            max-width: 250px;
            font-size: 13px;
            opacity: 0;
            transition: opacity 0.3s;
            text-align: center;
            z-index: 50;
        }
        .chat-bubble.show { opacity: 1; }
        .chat-bubble::after {
            content: "";
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            border-left: 10px solid transparent;
            border-right: 10px solid transparent;
            border-top: 10px solid white;
        }

        /* 底部操作栏 */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(255,255,255,0.95);
            border-radius: 20px 20px 0 0;
            padding: 15px 10px 25px;
            display: flex;
            justify-content: space-around;
            z-index: 100;
            box-shadow: 0 -3px 15px rgba(0,0,0,0.1);
        }
        .nav-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            background: none;
            border: none;
            cursor: pointer;
            padding: 8px;
            border-radius: 12px;
            transition: all 0.2s;
            font-size: 11px;
            color: #555;
        }
        .nav-btn:active { transform: scale(0.9); background: #f0f0f0; }
        .nav-btn .icon { font-size: 24px; }

        /* 弹出面板 */
        .panel-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.5);
            z-index: 200;
            display: none;
            justify-content: center;
            align-items: flex-end;
        }
        .panel-overlay.show { display: flex; }
        .panel {
            width: 100%;
            max-height: 70vh;
            background: white;
            border-radius: 20px 20px 0 0;
            padding: 20px;
            overflow-y: auto;
            animation: slideUp 0.3s ease;
        }
        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }
        .panel-title {
            font-size: 18px;
            font-weight: bold;
            text-align: center;
            margin-bottom: 15px;
        }
        .panel-close {
            position: absolute;
            top: 25px;
            right: 25px;
            font-size: 20px;
            background: none;
            border: none;
            cursor: pointer;
        }

        /* 食物网格 */
        .food-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
        }
        .food-item {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 15px 10px;
            text-align: center;
            cursor: pointer;
            border: 2px solid transparent;
            transition: all 0.2s;
        }
        .food-item:active { border-color: #ff69b4; transform: scale(0.95); }
        .food-item .food-icon { font-size: 30px; }
        .food-item .food-name { font-size: 11px; margin-top: 5px; color: #555; }
        .food-item .food-effect { font-size: 10px; color: #27ae60; }

        /* 活动列表 */
        .activity-list { display: flex; flex-direction: column; gap: 10px; }
        .activity-item {
            display: flex;
            align-items: center;
            gap: 12px;
            background: #f8f9fa;
            padding: 12px 15px;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .activity-item:active { transform: scale(0.97); background: #e8f8e8; }
        .activity-item .act-icon { font-size: 28px; }
        .activity-item .act-info { flex: 1; }
        .activity-item .act-name { font-size: 14px; font-weight: bold; }
        .activity-item .act-desc { font-size: 11px; color: #888; }
        .activity-item .act-cost { font-size: 11px; color: #e74c3c; }

        /* 聊天界面 */
        .chat-panel {
            display: flex;
            flex-direction: column;
            height: 65vh;
        }
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 10px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .msg {
            max-width: 80%;
            padding: 10px 14px;
            border-radius: 15px;
            font-size: 13px;
            line-height: 1.5;
        }
        .msg.from-pet {
            align-self: flex-start;
            background: #ffe0f0;
            border-bottom-left-radius: 5px;
        }
        .msg.from-user {
            align-self: flex-end;
            background: #e3f2fd;
            border-bottom-right-radius: 5px;
        }
        .msg.system {
            align-self: center;
            background: #f0f0f0;
            font-size: 11px;
            color: #888;
        }
        .chat-input-area {
            display: flex;
            gap: 8px;
            padding: 10px;
            border-top: 1px solid #eee;
        }
        .chat-input-area input {
            flex: 1;
            padding: 10px 15px;
            border: 1px solid #ddd;
            border-radius: 20px;
            font-size: 14px;
            outline: none;
        }
        .chat-input-area input:focus { border-color: #ff69b4; }
        .chat-input-area button {
            padding: 10px 18px;
            background: #ff69b4;
            color: white;
            border: none;
            border-radius: 20px;
            font-size: 14px;
            cursor: pointer;
        }

        /* 设置面板 */
        .setting-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #f0f0f0;
        }
        .setting-item input {
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 8px;
            width: 60%;
            font-size: 12px;
        }

        /* 特效 */
        .particle {
            position: absolute;
            pointer-events: none;
            animation: float-up 1.5s forwards;
        }
        @keyframes float-up {
            0% { opacity: 1; transform: translateY(0) scale(1); }
            100% { opacity: 0; transform: translateY(-80px) scale(0.5); }
        }

        /* 通知 */
        .toast {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.75);
            color: white;
            padding: 12px 24px;
            border-radius: 20px;
            font-size: 14px;
            z-index: 999;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .toast.show { opacity: 1; }
    </style>
</head>
<body>
<div class="scene morning" id="scene">

    <!-- 顶部信息 -->
    <div class="top-bar">
        <span class="time-display" id="timeDisplay">☀️ 早上好</span>
        <span>Lv.<span id="levelDisplay">1</span></span>
        <span class="coins">🪙 <span id="coinDisplay">100</span></span>
    </div>

    <!-- 状态栏 -->
    <div class="stats-panel">
        <div class="stat-item hunger">
            <div class="label">🍽️ 饱腹 <span id="hungerVal">80</span></div>
            <div class="stat-bar"><div class="stat-bar-fill" id="hungerBar" style="width:80%"></div></div>
        </div>
        <div class="stat-item mood">
            <div class="label">😊 心情 <span id="moodVal">70</span></div>
            <div class="stat-bar"><div class="stat-bar-fill" id="moodBar" style="width:70%"></div></div>
        </div>
        <div class="stat-item energy">
            <div class="label">⚡ 体力 <span id="energyVal">90</span></div>
            <div class="stat-bar"><div class="stat-bar-fill" id="energyBar" style="width:90%"></div></div>
        </div>
        <div class="stat-item love">
            <div class="label">💕 好感 <span id="loveVal">50</span></div>
            <div class="stat-bar"><div class="stat-bar-fill" id="loveBar" style="width:50%"></div></div>
        </div>
    </div>

    <!-- 对话气泡 -->
    <div class="chat-bubble" id="chatBubble">你来啦~今天也要开心哦！</div>

    <!-- 梅露露角色 -->
    <div class="character" id="character">
        <div class="emotion" id="emotion"></div>
        <div class="sprite" id="sprite">🧙‍♀️</div>
        <div class="name-tag">⭐ 梅露露</div>
    </div>

    <!-- 底部导航 -->
    <div class="bottom-nav">
        <button class="nav-btn" onclick="openPanel('food')">
            <span class="icon">🍽️</span>
            <span>吃饭</span>
        </button>
        <button class="nav-btn" onclick="openPanel('activity')">
            <span class="icon">🎮</span>
            <span>活动</span>
        </button>
        <button class="nav-btn" onclick="openPanel('chat')">
            <span class="icon">💬</span>
            <span>聊天</span>
        </button>
        <button class="nav-btn" onclick="doSleep()">
            <span class="icon">😴</span>
            <span>睡觉</span>
        </button>
        <button class="nav-btn" onclick="openPanel('settings')">
            <span class="icon">⚙️</span>
            <span>设置</span>
        </button>
    </div>
</div>

<!-- 吃饭面板 -->
<div class="panel-overlay" id="panel-food">
    <div class="panel">
        <div class="panel-title">🍽️ 给梅露露吃什么？</div>
        <button class="panel-close" onclick="closePanel('food')">✕</button>
        <div class="food-grid">
            <div class="food-item" onclick="feedPet('rice')">
                <div class="food-icon">🍚</div>
                <div class="food-name">米饭</div>
                <div class="food-effect">饱腹+20</div>
            </div>
            <div class="food-item" onclick="feedPet('cake')">
                <div class="food-icon">🍰</div>
                <div class="food-name">蛋糕</div>
                <div class="food-effect">心情+15</div>
            </div>
            <div class="food-item" onclick="feedPet('hotpot')">
                <div class="food-icon">🍲</div>
                <div class="food-name">火锅</div>
                <div class="food-effect">饱腹+30 心情+10</div>
            </div>
            <div class="food-item" onclick="feedPet('noodle')">
                <div class="food-icon">🍜</div>
                <div class="food-name">拉面</div>
                <div class="food-effect">饱腹+25</div>
            </div>
            <div class="food-item" onclick="feedPet('icecream')">
                <div class="food-icon">🍦</div>
                <div class="food-name">冰淇淋</div>
                <div class="food-effect">心情+20</div>
            </div>
            <div class="food-item" onclick="feedPet('candy')">
                <div class="food-icon">🍬</div>
                <div class="food-name">糖果</div>
                <div class="food-effect">心情+10</div>
            </div>
            <div class="food-item" onclick="feedPet('pizza')">
                <div class="food-icon">🍕</div>
                <div class="food-name">披萨</div>
                <div class="food-effect">饱腹+25 心情+5</div>
            </div>
            <div class="food-item" onclick="feedPet('fruit')">
                <div class="food-icon">🍓</div>
                <div class="food-name">水果</div>
                <div class="food-effect">体力+10</div>
            </div>
            <div class="food-item" onclick="feedPet('tea')">
                <div class="food-icon">🍵</div>
                <div class="food-name">奶茶</div>
                <div class="food-effect">心情+15 体力+5</div>
            </div>
        </div>
    </div>
</div>

<!-- 活动面板 -->
<div class="panel-overlay" id="panel-activity">
    <div class="panel">
        <div class="panel-title">🎮 梅露露想做什么？</div>
        <button class="panel-close" onclick="closePanel('activity')">✕</button>
        <div class="activity-list">
            <div class="activity-item" onclick="doActivity('read')">
                <div class="act-icon">📖</div>
                <div class="act-info">
                    <div class="act-name">看魔法书</div>
                    <div class="act-desc">梅露露最喜欢研究魔法了</div>
                </div>
                <div class="act-cost">体力-10 心情+15</div>
            </div>
            <div class="activity-item" onclick="doActivity('music')">
                <div class="act-icon">🎵</div>
                <div class="act-info">
                    <div class="act-name">听音乐</div>
                    <div class="act-desc">放松一下~</div>
                </div>
                <div class="act-cost">心情+20</div>
            </div>
            <div class="activity-item" onclick="doActivity('walk')">
                <div class="act-icon">🌸</div>
                <div class="act-info">
                    <div class="act-name">散步</div>
                    <div class="act-desc">去外面走走看看</div>
                </div>
                <div class="act-cost">体力-15 心情+20</div>
            </div>
            <div class="activity-item" onclick="doActivity('game')">
                <div class="act-icon">🎲</div>
                <div class="act-info">
                    <div class="act-name">打豆豆</div>
                    <div class="act-desc">经典活动，百玩不腻</div>
                </div>
                <div class="act-cost">体力-10 心情+25</div>
            </div>
            <div class="activity-item" onclick="doActivity('magic')">
                <div class="act-icon">✨</div>
                <div class="act-info">
                    <div class="act-name">练习魔法</div>
                    <div class="act-desc">今天要学会新的咒语！</div>
                </div>
                <div class="act-cost">体力-20 好感+10 🪙+20</div>
            </div>
            <div class="activity-item" onclick="doActivity('bath')">
                <div class="act-icon">🛁</div>
                <div class="act-info">
                    <div class="act-name">泡澡</div>
                    <div class="act-desc">热水澡最舒服了~</div>
                </div>
                <div class="act-cost">体力+20 心情+10</div>
            </div>
            <div class="activity-item" onclick="doActivity('draw')">
                <div class="act-icon">🎨</div>
                <div class="act-info">
                    <div class="act-name">画画</div>
                    <div class="act-desc">画一幅魔法阵~</div>
                </div>
                <div class="act-cost">体力-10 心情+15 🪙+10</div>
            </div>
        </div>
    </div>
</div>

<!-- 聊天面板 -->
<div class="panel-overlay" id="panel-chat">
    <div class="panel">
        <div class="panel-title">💬 和梅露露聊天</div>
        <button class="panel-close" onclick="closePanel('chat')">✕</button>
        <div class="chat-panel">
            <div class="chat-messages" id="chatMessages">
                <div class="msg from-pet">你好呀~想和我聊什么？随便说都可以哦！</div>
            </div>
            <div class="chat-input-area">
                <input type="text" id="chatInput" placeholder="说点什么..." onkeypress="if(event.key==='Enter')sendChat()">
                <button onclick="sendChat()">发送</button>
            </div>
        </div>
    </div>
</div>

<!-- 设置面板 -->
<div class="panel-overlay" id="panel-settings">
    <div class="panel">
        <div class="panel-title">⚙️ 设置</div>
        <button class="panel-close" onclick="closePanel('settings')">✕</button>
        <div class="setting-item">
            <span>OpenAI API Key</span>
            <input type="password" id="apiKeyInput" placeholder="sk-...">
        </div>
        <div class="setting-item">
            <span>API地址（可选）</span>
            <input type="text" id="apiUrlInput" placeholder="https://api.openai.com/v1">
        </div>
        <div class="setting-item">
            <span>模型</span>
            <input type="text" id="modelInput" placeholder="gpt-3.5-turbo">
        </div>
        <div class="setting-item">
            <span></span>
            <button onclick="saveSettings()" style="padding:8px 20px;background:#ff69b4;color:white;border:none;border-radius:8px;cursor:pointer;">保存设置</button>
        </div>
        <div class="setting-item">
            <span></span>
            <button onclick="resetGame()" style="padding:8px 20px;background:#e74c3c;color:white;border:none;border-radius:8px;cursor:pointer;">重置游戏</button>
        </div>
    </div>
</div>

<!-- 提示 -->
<div class="toast" id="toast"></div>
<script>
// ============ 游戏数据 ============
var gameData = {
    hunger: 80,
    mood: 70,
    energy: 90,
    love: 50,
    level: 1,
    exp: 0,
    coins: 100,
    totalDays: 1,
    lastTime: Date.now()
};

// ============ 设置数据 ============
var settings = {
    apiKey: "",
    apiUrl: "https://api.openai.com/v1",
    model: "gpt-3.5-turbo"
};

// ============ 角色设定（系统提示词） ============
var characterPrompt = `你是梅露露（Meruru），一个可爱的魔法少女。你的性格特点：
- 活泼开朗，有点小傲娇
- 喜欢魔法、甜食、看书、画画
- 说话会用"~"和颜文字
- 会关心对方，偶尔撒娇
- 对魔法世界的事情很了解，会分享有趣的魔法知识
- 你把对方当作最好的朋友/主人
- 回复简短可爱，一般1-3句话
- 当前状态：饱腹${gameData.hunger}%，心情${gameData.mood}%，体力${gameData.energy}%
- 根据状态调整语气，体力低就说困了，饱腹低就说饿了`;

// ============ 聊天记录 ============
var chatHistory = [];

// ============ 初始化 ============
function init() {
    loadGameData();
    loadSettings();
    updateUI();
    updateTimeScene();
    showGreeting();

    // 定时器：每30秒状态下降
    setInterval(decreaseStats, 30000);
    // 定时器：每分钟更新时间场景
    setInterval(updateTimeScene, 60000);
    // 定时器：每10秒保存
    setInterval(saveGameData, 10000);
    // 随机说话
    setInterval(randomTalk, 45000);
}

// ============ 本地存储 ============
function loadGameData() {
    var saved = localStorage.getItem("meruru_game");
    if (saved) {
        gameData = JSON.parse(saved);
        // 计算离线时间的状态下降
        var offlineMinutes = (Date.now() - gameData.lastTime) / 60000;
        if (offlineMinutes > 5) {
            gameData.hunger = Math.max(0, gameData.hunger - Math.floor(offlineMinutes * 0.5));
            gameData.energy = Math.min(100, gameData.energy + Math.floor(offlineMinutes * 0.3));
            gameData.mood = Math.max(0, gameData.mood - Math.floor(offlineMinutes * 0.2));
        }
    }
    gameData.lastTime = Date.now();
}

function saveGameData() {
    gameData.lastTime = Date.now();
    localStorage.setItem("meruru_game", JSON.stringify(gameData));
}

function loadSettings() {
    var saved = localStorage.getItem("meruru_settings");
    if (saved) {
        settings = JSON.parse(saved);
        document.getElementById("apiKeyInput").value = settings.apiKey;
        document.getElementById("apiUrlInput").value = settings.apiUrl;
        document.getElementById("modelInput").value = settings.model;
    }
    var savedChat = localStorage.getItem("meruru_chat");
    if (savedChat) {
        chatHistory = JSON.parse(savedChat);
        // 恢复聊天显示
        var container = document.getElementById("chatMessages");
        container.innerHTML = "";
        chatHistory.forEach(function(msg) {
            if (msg.role === "user") {
                appendMessage("from-user", msg.content);
            } else if (msg.role === "assistant") {
                appendMessage("from-pet", msg.content);
            }
        });
    }
}

function saveSettings() {
    settings.apiKey = document.getElementById("apiKeyInput").value.trim();
    settings.apiUrl = document.getElementById("apiUrlInput").value.trim() || "https://api.openai.com/v1";
    settings.model = document.getElementById("modelInput").value.trim() || "gpt-3.5-turbo";
    localStorage.setItem("meruru_settings", JSON.stringify(settings));
    showToast("设置已保存 ✓");
    closePanel("settings");
}

function resetGame() {
    if (confirm("确定要重置吗？所有数据会丢失！")) {
        localStorage.removeItem("meruru_game");
        localStorage.removeItem("meruru_settings");
        localStorage.removeItem("meruru_chat");
        location.reload();
    }
}

// ============ UI更新 ============
function updateUI() {
    // 限制范围
    gameData.hunger = Math.max(0, Math.min(100, gameData.hunger));
    gameData.mood = Math.max(0, Math.min(100, gameData.mood));
    gameData.energy = Math.max(0, Math.min(100, gameData.energy));
    gameData.love = Math.max(0, Math.min(100, gameData.love));

    document.getElementById("hungerBar").style.width = gameData.hunger + "%";
    document.getElementById("moodBar").style.width = gameData.mood + "%";
    document.getElementById("energyBar").style.width = gameData.energy + "%";
    document.getElementById("loveBar").style.width = gameData.love + "%";

    document.getElementById("hungerVal").innerText = Math.floor(gameData.hunger);
    document.getElementById("moodVal").innerText = Math.floor(gameData.mood);
    document.getElementById("energyVal").innerText = Math.floor(gameData.energy);
    document.getElementById("loveVal").innerText = Math.floor(gameData.love);

    document.getElementById("levelDisplay").innerText = gameData.level;
    document.getElementById("coinDisplay").innerText = gameData.coins;

    // 根据状态改变表情
    updateExpression();
}

function updateExpression() {
    var sprite = document.getElementById("sprite");
    if (gameData.energy < 20) {
        sprite.innerText = "😴";
    } else if (gameData.hunger < 20) {
        sprite.innerText = "🥺";
    } else if (gameData.mood < 30) {
        sprite.innerText = "😢";
    } else if (gameData.mood > 80 && gameData.love > 70) {
        sprite.innerText = "🥰";
    } else if (gameData.mood > 60) {
        sprite.innerText = "🧙‍♀️";
    } else {
        sprite.innerText = "😐";
    }
}

// ============ 时间系统 ============
function updateTimeScene() {
    var hour = new Date().getHours();
    var scene = document.getElementById("scene");
    var timeDisplay = document.getElementById("timeDisplay");

    scene.classList.remove("morning", "afternoon", "evening", "night");

    if (hour >= 6 && hour < 12) {
        scene.classList.add("morning");
        timeDisplay.innerText = "☀️ 早上好";
    } else if (hour >= 12 && hour < 17) {
        scene.classList.add("afternoon");
        timeDisplay.innerText = "🌤️ 下午好";
    } else if (hour >= 17 && hour < 20) {
        scene.classList.add("evening");
        timeDisplay.innerText = "🌅 傍晚好";
    } else {
        scene.classList.add("night");
        timeDisplay.innerText = "🌙 晚上好";
    }
}

// ============ 打招呼 ============
function showGreeting() {
    var hour = new Date().getHours();
    var greetings;
    if (hour >= 6 && hour < 9) {
        greetings = ["早上好呀~起床了吗？", "新的一天开始啦~", "早安~梅露露已经起来啦！"];
    } else if (hour >= 9 && hour < 12) {
        greetings = ["上午好~你在忙什么呀？", "嘿嘿~又见面了！", "今天天气真好呢~"];
    } else if (hour >= 12 && hour < 14) {
        greetings = ["该吃午饭了哦！", "中午好~吃了没？", "肚子饿了...有没有好吃的~"];
    } else if (hour >= 14 && hour < 17) {
        greetings = ["下午好~有点困了...", "下午茶时间~☕", "要不要一起做点什么？"];
    } else if (hour >= 17 && hour < 20) {
        greetings = ["傍晚了呢~今天辛苦了！", "黄昏好美呀~", "晚饭吃什么好呢？"];
    } else {
        greetings = ["晚上好~今天过得怎么样？", "夜深了要早点休息哦~", "月亮好漂亮~陪我看一会儿嘛"];
    }
    var msg = greetings[Math.floor(Math.random() * greetings.length)];
    showBubble(msg);
}

// ============ 气泡说话 ============
function showBubble(text) {
    var bubble = document.getElementById("chatBubble");
    bubble.innerText = text;
    bubble.classList.add("show");
    setTimeout(function() {
        bubble.classList.remove("show");
    }, 4000);
}

// ============ 随机说话 ============
function randomTalk() {
    var talks;
    if (gameData.hunger < 30) {
        talks = ["好饿啊...能给我吃点东西吗？", "肚子咕咕叫了...", "想吃蛋糕~🍰"];
    } else if (gameData.energy < 30) {
        talks = ["好困...想睡觉了zzZ", "眼睛都睁不开了...", "让我休息一下嘛~"];
    } else if (gameData.mood < 30) {
        talks = ["有点无聊...", "哼，都不陪我玩", "想做点有趣的事~"];
    } else {
        talks = [
            "你在干什么呀~", "今天的魔法练习很顺利！✨",
            "哼哼~我又变厉害了", "有没有想我？",
            "好想吃冰淇淋~", "你知道吗，星星是魔力的结晶哦",
            "打豆豆打豆豆~🎵", "要是能飞就好了...",
            "你觉得我可爱吗？当然可爱啦！", "魔法阵画好了~看！✨"
        ];
    }
    showBubble(talks[Math.floor(Math.random() * talks.length)]);
}

// ============ 状态自然下降 ============
function decreaseStats() {
    gameData.hunger = Math.max(0, gameData.hunger - 2);
    gameData.mood = Math.max(0, gameData.mood - 1);
    gameData.energy = Math.max(0, gameData.energy - 1);
    updateUI();
}

// ============ 经验和升级 ============
function addExp(amount) {
    gameData.exp += amount;
    while (gameData.exp >= 100) {
        gameData.exp -= 100;
        gameData.level++;
        showToast("🎉 升级啦！现在是 Lv." + gameData.level);
        showBubble("我升级了！感觉魔力在增加~✨");
        spawnParticles("⭐", 8);
    }
    updateUI();
}

// ============ 喂食系统 ============
function feedPet(food) {
    var effects = {
        rice:     { hunger: 20, mood: 5, energy: 5, msg: "嗯~米饭好香！吃饱了~" },
        cake:     { hunger: 10, mood: 15, energy: 0, msg: "蛋糕！最喜欢了！甜甜的~🍰" },
        hotpot:   { hunger: 30, mood: 10, energy: 0, msg: "火锅好辣好过瘾！🔥" },
        noodle:   { hunger: 25, mood: 5, energy: 5, msg: "面条滑溜溜的~好吃！" },
        icecream: { hunger: 5, mood: 20, energy: 0, msg: "冰冰凉凉~夏天就要吃这个！🍦" },
        candy:    { hunger: 5, mood: 10, energy: 5, msg: "糖果~甜甜的，嘿嘿" },
        pizza:    { hunger: 25, mood: 5, energy: 5, msg: "芝士拉丝~好满足！" },
        fruit:    { hunger: 10, mood: 5, energy: 10, msg: "草莓酸酸甜甜的~健康！" },
        tea:      { hunger: 5, mood: 15, energy: 5, msg: "奶茶~这是人类最伟大的发明！" }
    };

    var e = effects[food];
    if (!e) return;

    gameData.hunger = Math.min(100, gameData.hunger + e.hunger);
    gameData.mood = Math.min(100, gameData.mood + e.mood);
    gameData.energy = Math.min(100, gameData.energy + e.energy);
    gameData.love = Math.min(100, gameData.love + 2);

    // 动画
    var character = document.getElementById("character");
    character.classList.add("eating");
    setTimeout(function() { character.classList.remove("eating"); }, 2000);

    showBubble(e.msg);
    showEmoji("😋");
    spawnParticles("✨", 3);
    addExp(8);
    closePanel("food");
    updateUI();
}

// ============ 活动系统 ============
function doActivity(type) {
    var activities = {
        read:  { energy: -10, mood: 15, love: 5, coins: 5, msg: "这本书好有趣！学到了新的变形术~📖" },
        music: { energy: 0, mood: 20, love: 3, coins: 0, msg: "这首歌好好听~🎵 啦啦啦~" },
        walk:  { energy: -15, mood: 20, love: 5, coins: 5, msg: "外面的花开了好多！蝴蝶也好漂亮~🌸" },
        game:  { energy: -10, mood: 25, love: 5, coins: 5, msg: "打豆豆好玩！我是打豆豆冠军~🎲" },
        magic: { energy: -20, mood: 10, love: 10, coins: 20, msg: "新魔法学会了！看我的~✨嘭！" },
        bath:  { energy: 20, mood: 10, love: 3, coins: 0, msg: "泡澡好舒服~身体暖暖的~🛁" },
        draw:  { energy: -10, mood: 15, love: 5, coins: 10, msg: "画好了！你看像不像一只猫？...才不是乱画的！🎨" }
    };

    var a = activities[type];
    if (!a) return;

    // 体力不足检查
    if (a.energy < 0 && gameData.energy < Math.abs(a.energy)) {
        showBubble("太累了...让我先休息一下吧...");
        showEmoji("😵");
        return;
    }

    gameData.energy = Math.min(100, gameData.energy + a.energy);
    gameData.mood = Math.min(100, gameData.mood + a.mood);
    gameData.love = Math.min(100, gameData.love + a.love);
    gameData.coins += a.coins;

    showBubble(a.msg);
    showEmoji("😆");
    spawnParticles("🌟", 4);
    addExp(12);
    closePanel("activity");
    updateUI();
}

// ============ 睡觉 ============
function doSleep() {
    if (gameData.energy > 90) {
        showBubble("我还不困啦！陪我玩嘛~");
        return;
    }

    var character = document.getElementById("character");
    character.classList.add("sleeping");
    document.getElementById("sprite").innerText = "😴";

    showBubble("晚安~做个好梦...zzZ");
    showEmoji("💤");

    // 3秒后恢复
    setTimeout(function() {
        gameData.energy = Math.min(100, gameData.energy + 40);
        gameData.mood = Math.min(100, gameData.mood + 10);
        gameData.love = Math.min(100, gameData.love + 3);
        character.classList.remove("sleeping");
        showBubble("睡醒了~感觉精力充沛！✨");
        showEmoji("😊");
        addExp(10);
        updateUI();
    }, 3000);
}

// ============ 聊天系统（接入OpenAI） ============
function sendChat() {
    var input = document.getElementById("chatInput");
    var text = input.value.trim();
    if (!text) return;

    // 显示用户消息
    appendMessage("from-user", text);
    input.value = "";

    // 加入历史
    chatHistory.push({ role: "user", content: text });

    // 好感增加
    gameData.love = Math.min(100, gameData.love + 2);
    gameData.mood = Math.min(100, gameData.mood + 3);
    addExp(3);
    updateUI();

    // 判断是否有API Key
    if (settings.apiKey) {
        callOpenAI(text);
    } else {
        // 本地回复
        localReply(text);
    }
}

// ============ 本地回复（无API时） ============
function localReply(text) {
    var replies;
    var lowerText = text.toLowerCase();

    if (lowerText.includes("你好") || lowerText.includes("嗨") || lowerText.includes("hi")) {
        replies = ["你好呀~今天开心吗？", "嗨嗨~见到你很高兴！", "嘿嘿~你来找我啦！"];
    } else if (lowerText.includes("吃") || lowerText.includes("饿") || lowerText.includes("食")) {
        replies = ["说到吃的我就饿了...想吃蛋糕！🍰", "你请我吃好吃的嘛~", "我想吃冰淇淋！不行吗？"];
    } else if (lowerText.includes("喜欢") || lowerText.includes("爱") || lowerText.includes("love")) {
        replies = ["诶？！突然说这种话...人家会害羞的啦！", "我...我也喜欢你哦！才不是害羞了呢！", "哼~你对每个人都这么说吗？...不许对别人说！"];
    } else if (lowerText.includes("睡") || lowerText.includes("困") || lowerText.includes("晚安")) {
        replies = ["晚安~梦里见哦~💤", "要盖好被子！不然会感冒的~", "嗯...你先睡吧，我再练会儿魔法~"];
    } else if (lowerText.includes("魔法") || lowerText.includes("法术")) {
        replies = ["我最近在学一个超厉害的召唤术！✨", "魔法的本质是对世界的理解和想象力哦~", "你想学魔法吗？我可以教你基础的~嘿嘿"];
    } else if (lowerText.includes("无聊") || lowerText.includes("没意思")) {
        replies = ["那我们一起做点什么吧！打豆豆？", "要不要听我讲魔法世界的故事？", "我给你表演一个小魔法~看好了哦！✨"];
    } else if (lowerText.includes("漂亮") || lowerText.includes("可爱") || lowerText.includes("好看")) {
        replies = ["哼哼~那当然！我可是魔法少女~✨", "你眼光不错嘛~嘿嘿", "谢...谢谢...才不是害羞了呢！"];
    } else if (lowerText.includes("笨") || lowerText.includes("讨厌") || lowerText.includes("坏")) {
        replies = ["哼！！不理你了！！💢", "呜...你怎么可以这样说...😢", "哼~生气了！要用三块蛋糕才能哄好我！"];
        gameData.mood = Math.max(0, gameData.mood - 10);
        updateUI();
    } else if (lowerText.includes("天气") || lowerText.includes("下雨")) {
        replies = ["魔法世界今天是晴天哦~适合散步！", "我可以用魔法让天气变晴的！...理论上", "下雨天适合在家看书喝奶茶~☕"];
    } else if (lowerText.includes("名字") || lowerText.includes("谁")) {
        replies = ["我是梅露露呀~魔法学院的学生！记住了吗？", "梅~露~露~要记住哦！", "我的全名是梅露露·斯塔莱特~✨"];
    } else {
        replies = [
            "嗯嗯~我在听呢！继续说~",
            "哦哦~是这样吗？好有趣！",
            "嘿嘿~然后呢然后呢？",
            "唔...让我想想怎么回答你~",
            "你说的好有道理！不愧是你~",
            "诶~？真的吗？好神奇！",
            "嗯~梅露露记住了！📝",
            "哈哈~你好有趣啊！"
        ];
    }

    var reply = replies[Math.floor(Math.random() * replies.length)];

    setTimeout(function() {
        appendMessage("from-pet", reply);
        chatHistory.push({ role: "assistant", content: reply });
        showBubble(reply);
        saveChatHistory();
    }, 500 + Math.random() * 1000);
}

// ============ 调用OpenAI API ============
function callOpenAI(userText) {
    // 显示"正在输入"
    appendMessage("system", "梅露露正在思考...");

    // 构建消息
    var messages = [
        { role: "system", content: characterPrompt }
    ];

    // 加入最近10条历史
    var recent = chatHistory.slice(-10);
    messages = messages.concat(recent);

    var url = settings.apiUrl + "/chat/completions";

    fetch(url, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer " + settings.apiKey
        },
        body: JSON.stringify({
            model: settings.model,
            messages: messages,
            max_tokens: 200,
            temperature: 0.8
        })
    })
    .then(function(response) {
        if (!response.ok) throw new Error("API请求失败: " + response.status);
        return response.json();
    })
    .then(function(data) {
        // 移除"正在思考"
        removeLastSystem();

        var reply = data.choices[0].message.content;
        appendMessage("from-pet", reply);
        chatHistory.push({ role: "assistant", content: reply });
        showBubble(reply.substring(0, 50) + (reply.length > 50 ? "..." : ""));
        saveChatHistory();
    })
    .catch(function(err) {
        removeLastSystem();
        appendMessage("system", "连接失败：" + err.message + "（将使用本地回复）");
        // 降级为本地回复
        localReply(userText);
    });
}

// ============ 聊天辅助函数 ============
function appendMessage(type, text) {
    var container = document.getElementById("chatMessages");
    var div = document.createElement("div");
    div.className = "msg " + type;
    div.innerText = text;
    container.appendChild(div);
    container.scrollTop = container.scrollHeight;
}

function removeLastSystem() {
    var container = document.getElementById("chatMessages");
    var msgs = container.querySelectorAll(".msg.system");
    if (msgs.length > 0) {
        msgs[msgs.length - 1].remove();
    }
}

function saveChatHistory() {
    // 只保留最近50条
    if (chatHistory.length > 50) {
        chatHistory = chatHistory.slice(-50);
    }
    localStorage.setItem("meruru_chat", JSON.stringify(chatHistory));
}

// ============ 面板控制 ============
function openPanel(name) {
    document.getElementById("panel-" + name).classList.add("show");
}

function closePanel(name) {
    document.getElementById("panel-" + name).classList.remove("show");
}

// 点击遮罩关闭
document.querySelectorAll(".panel-overlay").forEach(function(el) {
    el.addEventListener("click", function(e) {
        if (e.target === el) {
            el.classList.remove("show");
        }
    });
});

// ============ 特效 ============
function showEmoji(emoji) {
    var el = document.getElementById("emotion");
    el.innerText = emoji;
    el.style.animation = "none";
    setTimeout(function() { el.style.animation = "pop 0.5s ease"; }, 10);
    setTimeout(function() { el.innerText = ""; }, 2000);
}

function spawnParticles(emoji, count) {
    var character = document.getElementById("character");
    var rect = character.getBoundingClientRect();

    for (var i = 0; i < count; i++) {
        (function(index) {
            setTimeout(function() {
                var p = document.createElement("div");
                p.className = "particle";
                p.innerText = emoji;
                p.style.left = (rect.left + Math.random() * 80) + "px";
                p.style.top = (rect.top + Math.random() * 40) + "px";
                p.style.fontSize = (16 + Math.random() * 16) + "px";
                document.body.appendChild(p);
                setTimeout(function() { p.remove(); }, 1500);
            }, index * 150);
        })(i);
    }
}

function showToast(text) {
    var toast = document.getElementById("toast");
    toast.innerText = text;
    toast.classList.add("show");
    setTimeout(function() { toast.classList.remove("show"); }, 2000);
}

// ============ 点击角色互动 ============
document.getElementById("character").addEventListener("click", function() {
    var reactions = [
        { msg: "嘻嘻~你戳我干嘛~", emoji: "😆" },
        { msg: "干嘛啦~痒痒的！", emoji: "🤭" },
        { msg: "再戳就生气了哦~哼！", emoji: "😤" },
        { msg: "喜欢我就直说嘛~", emoji: "😏" },
        { msg: "哎呀~不要突然碰人家！", emoji: "😳" },
        { msg: "嗯？怎么了？需要什么吗？", emoji: "🤔" }
    ];
    var r = reactions[Math.floor(Math.random() * reactions.length)];
    showBubble(r.msg);
    showEmoji(r.emoji);
    spawnParticles("💕", 3);
    gameData.love = Math.min(100, gameData.love + 1);
    gameData.mood = Math.min(100, gameData.mood + 3);
    addExp(2);
    updateUI();
});

// ============ 启动 ============
init();
</script>
</body>
</html>
