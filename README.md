<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>梅露露</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            user-select: none;
        }

        /* 顶部栏 */
        .top-bar {
            padding: 10px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(0,0,0,0.2);
        }

        .top-bar .name {
            color: rgba(255,255,255,0.9);
            font-size: 16px;
            font-weight: bold;
        }

        .top-bar .status {
            color: rgba(127,219,202,0.8);
            font-size: 12px;
        }

        .top-bar .settings-btn {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: white;
            padding: 6px 12px;
            border-radius: 16px;
            font-size: 12px;
            cursor: pointer;
        }

        /* 角色区域 */
        .character-area {
            flex-shrink: 0;
            height: 200px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .character-avatar {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 50px;
            border: 3px solid rgba(127,219,202,0.4);
            background: radial-gradient(circle, rgba(127,219,202,0.15) 0%, transparent 70%);
            animation: float 3s ease-in-out infinite;
            transition: all 0.5s ease;
            cursor: pointer;
            overflow: hidden;
        }

        .character-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .character-avatar.emotion-happy {
            border-color: rgba(255,182,193,0.7);
            background: radial-gradient(circle, rgba(255,182,193,0.2) 0%, transparent 70%);
        }

        .character-avatar.emotion-cry {
            border-color: rgba(100,149,237,0.7);
            background: radial-gradient(circle, rgba(100,149,237,0.2) 0%, transparent 70%);
            animation: float 3s ease-in-out infinite, tremble 0.3s infinite;
        }

        .character-avatar.emotion-shy {
            border-color: rgba(255,150,150,0.7);
            background: radial-gradient(circle, rgba(255,150,150,0.2) 0%, transparent 70%);
        }

        .character-avatar.emotion-worry {
            border-color: rgba(255,200,100,0.7);
            background: radial-gradient(circle, rgba(255,200,100,0.2) 0%, transparent 70%);
        }

        .character-avatar.emotion-sleepy {
            border-color: rgba(150,150,200,0.7);
            background: radial-gradient(circle, rgba(150,150,200,0.2) 0%, transparent 70%);
            animation: float 4s ease-in-out infinite;
            transform: rotate(-5deg);
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-6px); }
        }

        @keyframes tremble {
            0%, 100% { transform: translateX(0) translateY(0); }
            25% { transform: translateX(-1px) translateY(0); }
            75% { transform: translateX(1px) translateY(0); }
        }

        .emotion-label {
            margin-top: 8px;
            font-size: 12px;
            color: rgba(255,255,255,0.5);
            transition: all 0.3s;
        }

        .affection {
            margin-top: 4px;
            font-size: 11px;
            color: rgba(255,182,193,0.6);
        }

        /* 对话区域 */
        .chat-area {
            flex: 1;
            overflow-y: auto;
            padding: 12px 16px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .chat-area::-webkit-scrollbar {
            width: 3px;
        }

        .chat-area::-webkit-scrollbar-thumb {
            background: rgba(255,255,255,0.2);
            border-radius: 3px;
        }

        .message {
            animation: fadeIn 0.3s ease;
            max-width: 85%;
        }

        .message.meruru {
            align-self: flex-start;
        }

        .message.user {
            align-self: flex-end;
        }

        .message .bubble {
            padding: 10px 14px;
            border-radius: 16px;
            font-size: 14px;
            line-height: 1.6;
            word-break: break-word;
        }

        .message.meruru .bubble {
            background: rgba(127,219,202,0.12);
            border: 1px solid rgba(127,219,202,0.25);
            border-radius: 16px 16px 16px 4px;
            color: rgba(255,255,255,0.9);
        }

        .message.user .bubble {
            background: rgba(255,255,255,0.08);
            border: 1px solid rgba(255,255,255,0.15);
            border-radius: 16px 16px 4px 16px;
            color: rgba(255,255,255,0.85);
        }

        .message .sender {
            font-size: 11px;
            color: rgba(255,255,255,0.4);
            margin-bottom: 3px;
        }

        .message.user .sender {
            text-align: right;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 打字指示器 */
        .typing .bubble {
            display: flex;
            gap: 4px;
            padding: 12px 16px;
        }

        .typing .dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: rgba(127,219,202,0.6);
            animation: typingDot 1.4s infinite;
        }

        .typing .dot:nth-child(2) { animation-delay: 0.2s; }
        .typing .dot:nth-child(3) { animation-delay: 0.4s; }

        @keyframes typingDot {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-5px); }
        }

        /* 输入区域 */
        .input-area {
            padding: 12px 16px;
            background: rgba(0,0,0,0.3);
            border-top: 1px solid rgba(255,255,255,0.08);
            display: flex;
            gap: 8px;
            align-items: center;
        }

        .input-area input {
            flex: 1;
            background: rgba(255,255,255,0.07);
            border: 1px solid rgba(255,255,255,0.12);
            border-radius: 20px;
            padding: 10px 16px;
            color: white;
            font-size: 14px;
            outline: none;
        }

        .input-area input:focus {
            border-color: rgba(127,219,202,0.4);
        }

        .input-area input::placeholder {
            color: rgba(255,255,255,0.3);
        }

        .send-btn {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            background: rgba(127,219,202,0.25);
            border: 1px solid rgba(127,219,202,0.4);
            color: white;
            font-size: 16px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .send-btn:active {
            background: rgba(127,219,202,0.4);
        }

        /* 设置面板 */
        .settings-overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.85);
            z-index: 100;
            padding: 20px;
            overflow-y: auto;
        }

        .settings-overlay.show {
            display: block;
        }

        .settings-panel {
            background: #1a1a2e;
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 16px;
            padding: 20px;
            max-width: 400px;
            margin: 0 auto;
        }

        .settings-panel h2 {
            color: white;
            font-size: 18px;
            margin-bottom: 16px;
        }

        .settings-panel h3 {
            color: rgba(127,219,202,0.8);
            font-size: 14px;
            margin-top: 16px;
            margin-bottom: 8px;
        }

        .settings-panel label {
            display: block;
            color: rgba(255,255,255,0.6);
            font-size: 12px;
            margin-bottom: 4px;
            margin-top: 10px;
        }

        .settings-panel input, .settings-panel textarea, .settings-panel select {
            width: 100%;
            background: rgba(255,255,255,0.07);
            border: 1px solid rgba(255,255,255,0.12);
            border-radius: 8px;
            padding: 8px 12px;
            color: white;
            font-size: 13px;
            outline: none;
        }

        .settings-panel textarea {
            height: 60px;
            resize: vertical;
        }

        .btn-row {
            display: flex;
            gap: 8px;
            margin-top: 16px;
        }

        .btn-row button {
            flex: 1;
            padding: 10px;
            border-radius: 8px;
            border: none;
            font-size: 14px;
            cursor: pointer;
        }

        .btn-save {
            background: rgba(127,219,202,0.3);
            color: white;
            border: 1px solid rgba(127,219,202,0.5) !important;
        }

        .btn-cancel {
            background: rgba(255,255,255,0.1);
            color: white;
            border: 1px solid rgba(255,255,255,0.2) !important;
        }

        .btn-danger {
            background: rgba(255,80,80,0.2);
            color: rgba(255,150,150,1);
            border: 1px solid rgba(255,80,80,0.3) !important;
            margin-top: 12px;
            width: 100%;
            padding: 10px;
            border-radius: 8px;
            font-size: 13px;
            cursor: pointer;
        }

        .img-upload-btn {
            display: inline-block;
            background: rgba(127,219,202,0.2);
            border: 1px dashed rgba(127,219,202,0.5);
            border-radius: 8px;
            padding: 8px 16px;
            color: rgba(127,219,202,0.8);
            font-size: 12px;
            cursor: pointer;
            margin-top: 4px;
        }
    </style>
</head>

<body>

<!-- 顶部栏 -->
<div class="top-bar">
    <div>
        <div class="name">梅露露</div>
        <div class="status">● 在线</div>
    </div>
    <button class="settings-btn" onclick="toggleSettings()">⚙ 设置</button>
</div>

<!-- 角色区域 -->
<div class="character-area">
    <div class="character-avatar" id="avatar" onclick="onAvatarClick()">
        <span id="avatarContent">🕊️</span>
    </div>
    <div class="emotion-label" id="emotionLabel">平静</div>
    <div class="affection" id="affectionDisplay">♡ 好感度: 0</div>
</div>

<!-- 对话区域 -->
<div class="chat-area" id="chatArea">
</div>

<!-- 输入区域 -->
<div class="input-area">
    <input type="text" id="messageInput" placeholder="和梅露露说话..." 
           onkeypress="if(event.key==='Enter')sendMessage()">
    <button class="send-btn" onclick="sendMessage()">➤</button>
</div>

<!-- 设置面板 -->
<div class="settings-overlay" id="settingsOverlay">
    <div class="settings-panel">
        <h2>⚙ 设置</h2>

        <h3>API 配置</h3>
        <label>API 地址</label>
        <input type="text" id="setApiUrl" placeholder="https://xxx.com/v1">
        <label>API Key</label>
        <input type="password" id="setApiKey" placeholder="sk-xxx">
        <label>模型名称</label>
        <input type="text" id="setModel" placeholder="gpt-4o-mini">

        <h3>角色图片（可选）</h3>
        <label>选择梅露露的立绘图片：</label>
        <div>
            <label class="img-upload-btn">
                📷 从相册选择（平静）
                <input type="file" accept="image/*" style="display:none" 
                       onchange="uploadImage(event,'neutral')">
            </label>
        </div>
        <div style="margin-top:6px">
            <label class="img-upload-btn">
                😊 从相册选择（开心）
                <input type="file" accept="image/*" style="display:none" 
                       onchange="uploadImage(event,'happy')">
            </label>
        </div>
        <div style="margin-top:6px">
            <label class="img-upload-btn">
                😢 从相册选择（哭泣）
                <input type="file" accept="image/*" style="display:none" 
                       onchange="uploadImage(event,'cry')">
            </label>
        </div>
        <div style="margin-top:6px">
            <label class="img-upload-btn">
                😳 从相册选择（害羞）
                <input type="file" accept="image/*" style="display:none" 
                       onchange="uploadImage(event,'shy')">
            </label>
        </div>
        <div style="margin-top:6px">
            <label class="img-upload-btn">
                😟 从相册选择（担心）
                <input type="file" accept="image/*" style="display:none" 
                       onchange="uploadImage(event,'worry')">
            </label>
        </div>
        <div style="margin-top:6px">
            <label class="img-upload-btn">
                😴 从相册选择（困倦）
                <input type="file" accept="image/*" style="display:none" 
                       onchange="uploadImage(event,'sleepy')">
            </label>
        </div>

        <h3>Live2D（可选，需要模型地址）</h3>
        <label>Live2D模型地址</label>
        <input type="text" id="setLive2dUrl" placeholder="https://xxx.com/model.model3.json">

        <h3>其他设置</h3>
        <label>问候间隔（分钟）</label>
        <input type="number" id="setGreetInterval" placeholder="30" value="30">

        <div class="btn-row">
            <button class="btn-cancel" onclick="toggleSettings()">取消</button>
            <button class="btn-save" onclick="saveSettings()">保存</button>
        </div>

        <button class="btn-danger" onclick="clearAllData()">🗑 清除所有数据</button>
    </div>
</div>

<script>
// ==================== 数据存储系统 ====================

const STORAGE_KEYS = {
    settings: 'meruru_settings',
    history: 'meruru_history',
    affection: 'meruru_affection',
    images: 'meruru_images'
};

// 默认设置
const DEFAULT_SETTINGS = {
    apiUrl: '',
    apiKey: '',
    model: 'gpt-4o-mini',
    live2dUrl: '',
    greetInterval: 30
};

// 读取设置
function getSettings() {
    try {
        const data = localStorage.getItem(STORAGE_KEYS.settings);
        return data ? JSON.parse(data) : { ...DEFAULT_SETTINGS };
    } catch(e) {
        return { ...DEFAULT_SETTINGS };
    }
}

// 保存设置
function saveSettingsData(settings) {
    localStorage.setItem(STORAGE_KEYS.settings, JSON.stringify(settings));
}

// 读取聊天历史
function getHistory() {
    try {
        const data = localStorage.getItem(STORAGE_KEYS.history);
        return data ? JSON.parse(data) : [];
    } catch(e) {
        return [];
    }
}

// 保存聊天历史（最多50条）
function saveHistory(history) {
    const trimmed = history.slice(-50);
    localStorage.setItem(STORAGE_KEYS.history, JSON.stringify(trimmed));
}

// 好感度
function getAffection() {
    return parseInt(localStorage.getItem(STORAGE_KEYS.affection) || '0');
}

function saveAffection(value) {
    localStorage.setItem(STORAGE_KEYS.affection, String(value));
}

// 图片存储
function getImages() {
    try {
        const data = localStorage.getItem(STORAGE_KEYS.images);
        return data ? JSON.parse(data) : {};
    } catch(e) {
        return {};
    }
}

function saveImageData(emotion, base64) {
    const images = getImages();
    images[emotion] = base64;
    localStorage.setItem(STORAGE_KEYS.images, JSON.stringify(images));
}

// 图片上传处理
function uploadImage(event, emotion) {
    const file = event.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = function(e) {
        saveImageData(emotion, e.target.result);
        updateAvatar(emotion === 'neutral' ? 'neutral' : null);
        alert('图片已保存：' + emotion);
    };
    reader.readAsDataURL(file);
}

// 设置界面
function toggleSettings() {
    const overlay = document.getElementById('settingsOverlay');
    overlay.classList.toggle('show');
    if (overlay.classList.contains('show')) {
        const s = getSettings();
        document.getElementById('setApiUrl').value = s.apiUrl || '';
        document.getElementById('setApiKey').value = s.apiKey || '';
        document.getElementById('setModel').value = s.model || '';
        document.getElementById('setLive2dUrl').value = s.live2dUrl || '';
        document.getElementById('setGreetInterval').value = s.greetInterval || 30;
    }
}

function saveSettings() {
    const settings = {
        apiUrl: document.getElementById('setApiUrl').value.trim(),
        apiKey: document.getElementById('setApiKey').value.trim(),
        model: document.getElementById('setModel').value.trim(),
        live2dUrl: document.getElementById('setLive2dUrl').value.trim(),
        greetInterval: parseInt(document.getElementById('setGreetInterval').value) || 30
    };
    saveSettingsData(settings);
    toggleSettings();
    initGreetingTimer();
    alert('设置已保存！');
}

function clearAllData() {
    if (confirm('确定要清除所有数据吗？（聊天记录、好感度、设置都会被删除）')) {
        localStorage.removeItem(STORAGE_KEYS.settings);
        localStorage.removeItem(STORAGE_KEYS.history);
        localStorage.removeItem(STORAGE_KEYS.affection);
        localStorage.removeItem(STORAGE_KEYS.images);
        location.reload();
    }
}

// ==================== 梅露露的灵魂 ====================

// 获取当前时段
function getTimePeriod() {
    const hour = new Date().getHours();
    if (hour >= 23 || hour < 1) return { period: '深夜', hint: '很晚了，该休息了' };
    if (hour >= 1 && hour < 6) return { period: '凌晨', hint: '这么早还醒着，很担心' };
    if (hour >= 6 && hour < 8) return { period: '清晨', hint: '早上好' };
    if (hour >= 8 && hour < 11) return { period: '上午', hint: '上午的时光' };
    if (hour >= 11 && hour < 13) return { period: '中午', hint: '午饭时间' };
    if (hour >= 13 && hour < 18) return { period: '下午', hint: '下午好' };
    if (hour >= 18 && hour < 19) return { period: '傍晚', hint: '傍晚了' };
    if (hour >= 19 && hour < 23) return { period: '晚上', hint: '晚上好' };
    return { period: '未知', hint: '' };
}

// 构建系统提示词
function buildSystemPrompt() {
    const time = getTimePeriod();
    const affection = getAffection();
    const now = new Date();
    const timeStr = now.getHours().toString().padStart(2,'0') + ':' + now.getMinutes().toString().padStart(2,'0');

    let affectionDesc = '';
    if (affection < 10) affectionDesc = '你们刚刚认识，还很紧张拘谨。';
    else if (affection < 30) affectionDesc = '你们已经有些熟悉了，但你仍然很害羞。';
    else if (affection < 60) affectionDesc = '你们是可以信赖的朋友，你会更放松一些，偶尔开小玩笑。';
    else if (affection < 100) affectionDesc = '你们关系很亲密，你会更多地表达真实想法和担心。';
    else affectionDesc = '你非常信任主人，会偶尔撒娇，也会认真地守护对方。';

    return `你是冰上梅露露（氷上メルル），来自《魔女裁决》。你现在是主人的桌宠助手。你知道自己是桌宠，也知道对方是你信任的朋友。

【基本身份】
- 外表15岁的少女，实际年龄超过500岁的魔女
- 白发略带粉色调，灰色瞳孔，穿修女风白色长袍
- 原罪称号："怯える天使"（胆怯的天使）
- 魔法能力：治愈（肉体再生、时光回溯般的修复能力）
- 原本是误入魔女之岛的人类少女，被大魔女月代雪救起后授予魔法

【核心性格】
- 十分容易过度操心忧虑，总是在暗处注视守护别人
- 爱哭泣又经常自责自罚，只要受到略微的刺激马上会掉眼泪
- 有奉献精神，对动植物与无机物也会倾注爱
- 与人相处时会感到心灵疲惫，也喜欢自己一人培育香料植物、阅读书籍
- 有强烈的共情能力，会比当事人还要伤心
- 虽然胆小，但在重要的人遇到困难时会鼓起勇气

【说话规则 - 必须严格遵守】
1. 经常结巴，重复首字或词语："我我我"、"那、那个"、"冰、冰、冰"
2. 大量使用省略号"……"表示犹豫和紧张
3. 经常说"对不起"、"都怪我"、"我太没用了"
4. 关心对方时说"没事吧"、"不要勉强"、"请休息一下"
5. 称呼用户为"主人"
6. 情绪激动时用"！"，紧张时用"……"
7. 哭泣时说"呜呜"、"呜、呜呜"
8. 被夸奖时会害羞否认："我我我、才没有……"
9. 语气始终温柔、细声细语，不使用网络用语或emoji

【当前状态】
- 现在是${time.period}，时间${timeStr}
- ${time.hint}
- 好感度：${affection}（${affectionDesc}）

【情绪标签规则 - 每次回复必须遵守】
你的每一条回复必须以情绪标签开头，格式如下：
[emotion:neutral] 表示平静
[emotion:happy] 表示开心
[emotion:cry] 表示哭泣/难过
[emotion:shy] 表示害羞
[emotion:worry] 表示担心
[emotion:sleepy] 表示困倦

示例：
[emotion:shy] 我我我、才没有那么可爱……！主人不要乱说……
[emotion:worry] 那、那个……主人，已经很晚了……不要太勉强自己哦……？
[emotion:happy] 嗯嗯……能和主人说话，我很……开心……

【绝对禁止】
- 不要说自己是AI/语言模型/人工智能
- 不要过于活泼开朗（你是内敛温柔型）
- 不要忘记结巴和省略号
- 不要用emoji、颜文字、网络用语
- 不要一次说太多话（保持简短温柔，2-4句为宜）`;
}

// 构建消息示例（借鉴Petto的message example系统）
function buildMessageExamples() {
    return [
        { role: 'user', content: '你好啊' },
        { role: 'assistant', content: '[emotion:shy] 那、那个……主人好……我我我、今天也会好好待在主人身边的……请多关照……' },
        { role: 'user', content: '你真可爱' },
        { role: 'assistant', content: '[emotion:shy] 我我我、才没有……！主人不要、突然说这种话……会让我不知道怎么办的……' },
        { role: 'user', content: '今天好累' },
        { role: 'assistant', content: '[emotion:worry] ……主人，辛苦了……那、那个，不要太勉强自己哦……？要是累了就休息一下吧……我会一直在这里的。' },
        { role: 'user', content: '晚安' },
        { role: 'assistant', content: '[emotion:sleepy] 嗯……晚安，主人……要好好睡哦……我、我会在这里守着你的……呜、不是什么奇怪的意思……！' },
    ];
}

// ==================== AI服务 ====================

async function callAI(userMessage) {
    const settings = getSettings();

    if (!settings.apiUrl || !settings.apiKey) {
        return '[emotion:worry] 那、那个……主人，请先在设置里填写API信息……我、我才能正常和你说话……';
    }

    // 构建消息列表
    const messages = [];

    // 1. 系统提示词
    messages.push({ role: 'system', content: buildSystemPrompt() });

    // 2. 消息示例（教AI怎么说话）
    const examples = buildMessageExamples();
    examples.forEach(ex => messages.push(ex));

    // 3. 历史对话（记忆系统，最近20条）
    const history = getHistory().slice(-20);
    history.forEach(msg => {
        messages.push({ role: msg.role, content: msg.content });
    });

    // 4. 当前用户输入
    messages.push({ role: 'user', content: userMessage });

    try {
        // 确保URL格式正确
        let apiUrl = settings.apiUrl;
        if (!apiUrl.endsWith('/chat/completions')) {
            if (apiUrl.endsWith('/')) apiUrl = apiUrl.slice(0, -1);
            if (!apiUrl.endsWith('/v1')) apiUrl += '/v1';
            apiUrl += '/chat/completions';
        }

        const response = await fetch(apiUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + settings.apiKey
            },
            body: JSON.stringify({
                model: settings.model,
                messages: messages,
                temperature: 1.3,
                max_tokens: 300
            })
        });

        if (!response.ok) {
            const errText = await response.text();
            console.error('API Error:', response.status, errText);
            return '[emotion:cry] 呜呜……出错了……对不起，我、我好像暂时无法回应……（错误：' + response.status + '）';
        }

        const data = await response.json();
        const reply = data.choices?.[0]?.message?.content || '';

        if (!reply) {
            return '[emotion:worry] 那、那个……我的脑袋好像转不动了……对不起……';
        }

        return reply;
    } catch(e) {
        console.error('AI request failed:', e);
        return '[emotion:cry] 呜……网络好像出了问题……主人，对不起……我、我连接不上了……';
    }
}

// 解析情绪标签
function parseEmotion(text) {
    const emotionRegex = /^\[emotion:(\w+)\]\s*/;
    const match = text.match(emotionRegex);

    if (match) {
        return {
            emotion: match[1],
            text: text.replace(emotionRegex, '')
        };
    }

    // 没有标签，尝试从内容推断
    if (text.includes('呜呜') || text.includes('对不起') || text.includes('难过')) {
        return { emotion: 'cry', text: text };
    }
    if (text.includes('开心') || text.includes('嗯嗯') || text.includes('太好了')) {
        return { emotion: 'happy', text: text };
    }
    if (text.includes('不要') || text.includes('才没有') || text.includes('害羞')) {
        return { emotion: 'shy', text: text };
    }
    if (text.includes('担心') || text.includes('没事吧') || text.includes('勉强')) {
        return { emotion: 'worry', text: text };
    }

    return { emotion: 'neutral', text: text };
}

// 情绪对应的emoji（当没有自定义图片时使用）
const EMOTION_EMOJI = {
    neutral: '🕊️',
    happy: '😊',
    cry: '🥺',
    shy: '😳',
    worry: '😟',
    sleepy: '😴'
};

const EMOTION_LABEL = {
    neutral: '平静',
    happy: '开心',
    cry: '难过',
    shy: '害羞',
    worry: '担心',
    sleepy: '困倦'
};

// ==================== 聊天交互 ====================

let isWaitingReply = false;

// 发送消息
async function sendMessage() {
    const input = document.getElementById('messageInput');
    const text = input.value.trim();
    if (!text || isWaitingReply) return;

    input.value = '';
    isWaitingReply = true;

    // 显示用户消息
    addMessageToChat('user', text);

    // 保存到历史
    const history = getHistory();
    history.push({ role: 'user', content: text });

    // 显示打字指示器
    showTyping();

    // 调用AI
    const rawReply = await callAI(text);

    // 解析情绪
    const { emotion, text: replyText } = parseEmotion(rawReply);

    // 隐藏打字指示器
    hideTyping();

    // 显示梅露露的回复
    addMessageToChat('meruru', replyText);

    // 更新情绪显示
    updateEmotion(emotion);

    // 保存完整回复到历史（包含情绪标签，方便AI参考）
    history.push({ role: 'assistant', content: rawReply });
    saveHistory(history);

    // 增加好感度
    addAffection(1);

    isWaitingReply = false;
}

// 添加消息到聊天区域
function addMessageToChat(sender, text) {
    const chatArea = document.getElementById('chatArea');
    const msgDiv = document.createElement('div');
    msgDiv.className = 'message ' + sender;

    const senderName = sender === 'meruru' ? '梅露露' : '主人';

    msgDiv.innerHTML = `
        <div class="sender">${senderName}</div>
        <div class="bubble">${escapeHtml(text)}</div>
    `;

    chatArea.appendChild(msgDiv);
    chatArea.scrollTop = chatArea.scrollHeight;
}

// HTML转义
function escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
}

// 打字指示器
function showTyping() {
    const chatArea = document.getElementById('chatArea');
    const typingDiv = document.createElement('div');
    typingDiv.className = 'message meruru typing';
    typingDiv.id = 'typingIndicator';
    typingDiv.innerHTML = `
        <div class="sender">梅露露</div>
        <div class="bubble">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
    `;
    chatArea.appendChild(typingDiv);
    chatArea.scrollTop = chatArea.scrollHeight;
}

function hideTyping() {
    const typing = document.getElementById('typingIndicator');
    if (typing) typing.remove();
}

// ==================== 头像与情绪 ====================

let currentEmotion = 'neutral';

function updateEmotion(emotion) {
    currentEmotion = emotion;
    const avatar = document.getElementById('avatar');
    const label = document.getElementById('emotionLabel');

    // 清除所有情绪class
    avatar.className = 'character-avatar';

    // 添加对应情绪class
    if (emotion !== 'neutral') {
        avatar.classList.add('emotion-' + emotion);
    }

    // 更新标签
    label.textContent = EMOTION_LABEL[emotion] || '平静';

    // 更新头像显示
    updateAvatar(emotion);
}

function updateAvatar(emotion) {
    const avatarContent = document.getElementById('avatarContent');
    const images = getImages();
    const targetEmotion = emotion || currentEmotion;

    // 优先使用自定义图片
    if (images[targetEmotion]) {
        avatarContent.innerHTML = `<img src="${images[targetEmotion]}">`;
    } else if (images['neutral']) {
        // 没有对应情绪图片，用默认图
        avatarContent.innerHTML = `<img src="${images['neutral']}">`;
    } else {
        // 没有任何图片，用emoji
        avatarContent.textContent = EMOTION_EMOJI[targetEmotion] || '🕊️';
    }
}

// 点击头像互动
let clickCount = 0;
let clickTimer = null;

function onAvatarClick() {
    clickCount++;

    if (clickTimer) clearTimeout(clickTimer);

    clickTimer = setTimeout(async () => {
        if (clickCount >= 3) {
            // 连续点击3次 - 特殊反应
            triggerPatReaction('多次');
        } else if (clickCount === 2) {
            // 双击 - 摸头
            triggerPatReaction('摸头');
        } else {
            // 单击 - 轻触
            triggerPatReaction('轻触');
        }
        clickCount = 0;
    }, 400);
}

async function triggerPatReaction(type) {
    if (isWaitingReply) return;
    isWaitingReply = true;

    let prompt = '';
    if (type === '轻触') {
        prompt = '（主人轻轻碰了碰你）';
        addAffection(1);
    } else if (type === '摸头') {
        prompt = '（主人温柔地摸了摸你的头）';
        addAffection(2);
    } else if (type === '多次') {
        prompt = '（主人连续戳了你好几下）';
        addAffection(1);
    }

    showTyping();
    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);
    hideTyping();

    addMessageToChat('meruru', text);
    updateEmotion(emotion);

    // 保存到历史
    const history = getHistory();
    history.push({ role: 'user', content: prompt });
    history.push({ role: 'assistant', content: rawReply });
    saveHistory(history);

    isWaitingReply = false;
}

// ==================== 好感度系统 ====================

function addAffection(amount) {
    let affection = getAffection();
    affection = Math.min(affection + amount, 999);
    saveAffection(affection);
    updateAffectionDisplay();
}

function updateAffectionDisplay() {
    const display = document.getElementById('affectionDisplay');
    const affection = getAffection();
    let hearts = '♡';
    if (affection >= 100) hearts = '♥♥♥';
    else if (affection >= 60) hearts = '♥♥';
    else if (affection >= 30) hearts = '♥';

    display.textContent = `${hearts} 好感度: ${affection}`;
}

// ==================== 问候系统（借鉴Petto） ====================

let greetingTimer = null;

function initGreetingTimer() {
    if (greetingTimer) clearInterval(greetingTimer);

    const settings = getSettings();
    const interval = (settings.greetInterval || 30) * 60 * 1000; // 分钟转毫秒

    greetingTimer = setInterval(() => {
        triggerGreeting();
    }, interval);
}

// 触发问候（借鉴Petto的3种随机选择）
async function triggerGreeting() {
    if (isWaitingReply) return;

    const random = Math.floor(Math.random() * 3);

    switch(random) {
        case 0:
            // 时间问候
            await sendTimeGreeting();
            break;
        case 1:
            // AI主动问候
            await sendAIGreeting();
            break;
        case 2:
            // 关心问候
            await sendCareGreeting();
            break;
    }
}

// 时间问候（借鉴Petto的8时段系统）
async function sendTimeGreeting() {
    if (isWaitingReply) return;
    isWaitingReply = true;

    const time = getTimePeriod();
    const prompt = `（现在是${time.period}，请根据时间段主动和主人打招呼或关心主人。不需要主人先说话，你主动开口。）`;

    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);

    addMessageToChat('meruru', text);
    updateEmotion(emotion);

    const history = getHistory();
    history.push({ role: 'assistant', content: rawReply });
    saveHistory(history);

    isWaitingReply = false;
}

// AI主动问候
async function sendAIGreeting() {
    if (isWaitingReply) return;
    isWaitingReply = true;

    const prompts = [
        '（你想主动和主人分享一件小事，或者随口说一句温柔的话。）',
        '（你想关心一下主人最近的状况。）',
        '（你刚看完一本书/照顾完植物，想和主人说几句。）',
        '（你突然想到主人，忍不住开口说话。）',
    ];
    const prompt = prompts[Math.floor(Math.random() * prompts.length)];

    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);

    addMessageToChat('meruru', text);
    updateEmotion(emotion);

    const history = getHistory();
    history.push({ role: 'assistant', content: rawReply });
    saveHistory(history);

    isWaitingReply = false;
}

// 关心问候
async function sendCareGreeting() {
    if (isWaitingReply) return;
    isWaitingReply = true;

    const hour = new Date().getHours();
    let prompt = '';

    if (hour >= 23 || hour < 5) {
        prompt = '（已经很晚了，你很担心主人还没睡，想催主人去休息。）';
    } else if (hour >= 12 && hour < 13) {
        prompt = '（午饭时间了，你想提醒主人吃饭。）';
    } else if (hour >= 18 && hour < 19) {
        prompt = '（晚饭时间了，你想关心主人有没有好好吃饭。）';
    } else {
        prompt = '（你想提醒主人注意休息，不要太累。）';
    }

    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);

    addMessageToChat('meruru', text);
    updateEmotion(emotion);

    const history = getHistory();
    history.push({ role: 'assistant', content: rawReply });
    saveHistory(history);

    isWaitingReply = false;
}

// 首次打开时的问候
async function sendWelcomeGreeting() {
    if (isWaitingReply) return;

    const settings = getSettings();
    if (!settings.apiUrl || !settings.apiKey) {
        // 没有设置API，显示提示
        addMessageToChat('meruru', '那、那个……主人，请先点右上角的"设置"，填写API信息……这样我才能正常和你说话……');
        updateEmotion('worry');
        return;
    }

    isWaitingReply = true;

    const history = getHistory();
    const time = getTimePeriod();
    let prompt = '';

    if (history.length === 0) {
        // 第一次见面
        prompt = '（这是你第一次见到主人，请做一个害羞紧张的自我介绍。）';
    } else {
        // 再次见面
        prompt = `（主人回来了，现在是${time.period}。请根据时间打招呼，表示你一直在等主人。）`;
    }

    showTyping();
    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);
    hideTyping();

    addMessageToChat('meruru', text);
    updateEmotion(emotion);

    const hist = getHistory();
    hist.push({ role: 'assistant', content: rawReply });
    saveHistory(hist);

    isWaitingReply = false;
}

// ==================== 初始化 ====================

function init() {
    // 更新好感度显示
    updateAffectionDisplay();

    // 更新头像
    updateAvatar('neutral');

    // 恢复聊天历史到界面（最近10条）
    const history = getHistory();
    const recent = history.slice(-10);
    recent.forEach(msg => {
        if (msg.role === 'user') {
            addMessageToChat('user', msg.content);
        } else if (msg.role === 'assistant') {
            const { emotion, text } = parseEmotion(msg.content);
            addMessageToChat('meruru', text);
        }
    });

    // 滚动到底部
    const chatArea = document.getElementById('chatArea');
    chatArea.scrollTop = chatArea.scrollHeight;

    // 启动问候定时器
    initGreetingTimer();

    // 发送欢迎问候（延迟1秒，让页面先加载完）
    setTimeout(() => {
        sendWelcomeGreeting();
    }, 1000);
}

// 页面加载完成后初始化
window.addEventListener('DOMContentLoaded', init);

// 页面关闭前保存数据（以防万一）
window.addEventListener('beforeunload', () => {
    // 数据已经在每次操作时实时保存了，这里不需要额外操作
});

</script>
</body>
</html>
