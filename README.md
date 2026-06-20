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
            -webkit-tap-highlight-color: transparent;
        }

        html, body {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background: transparent;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }

        /* Live2D 画布 - 占满整个屏幕 */
        #live2d-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* 背景（可后期改透明用于悬浮窗） */
        #bg-layer {
            position: fixed;
            inset: 0;
            z-index: 0;
            background: linear-gradient(160deg, #1a1a2e 0%, #16213e 55%, #0f3460 100%);
        }

        /* 顶部按钮栏 */
        .top-buttons {
            position: fixed;
            top: 12px;
            right: 12px;
            z-index: 20;
            display: flex;
            gap: 8px;
        }

        .top-buttons button {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(0,0,0,0.35);
            border: 1px solid rgba(255,255,255,0.2);
            color: rgba(255,255,255,0.85);
            font-size: 16px;
            cursor: pointer;
            backdrop-filter: blur(4px);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .top-buttons button:active {
            background: rgba(127,219,202,0.4);
        }

        /* 好感度显示 */
        .affection-badge {
            position: fixed;
            top: 14px;
            left: 12px;
            z-index: 20;
            background: rgba(0,0,0,0.35);
            border: 1px solid rgba(255,182,193,0.3);
            border-radius: 16px;
            padding: 6px 12px;
            color: rgba(255,182,193,0.9);
            font-size: 12px;
            backdrop-filter: blur(4px);
        }

        /* 说话气泡（桌宠核心：出现后自动消失） */
        .speech-bubble {
            position: fixed;
            left: 50%;
            top: 18%;
            transform: translateX(-50%) translateY(10px);
            z-index: 15;
            max-width: 80%;
            min-width: 120px;
            background: rgba(255,255,255,0.95);
            color: #2a2a3e;
            padding: 14px 18px;
            border-radius: 18px;
            font-size: 15px;
            line-height: 1.6;
            box-shadow: 0 6px 24px rgba(0,0,0,0.3);
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s ease, transform 0.4s ease;
        }

        .speech-bubble.show {
            opacity: 1;
            transform: translateX(-50%) translateY(0);
        }

        /* 气泡小尾巴 */
        .speech-bubble::after {
            content: '';
            position: absolute;
            bottom: -8px;
            left: 50%;
            transform: translateX(-50%);
            border-left: 10px solid transparent;
            border-right: 10px solid transparent;
            border-top: 10px solid rgba(255,255,255,0.95);
        }
        /* 底部输入栏（可隐藏） */
        .input-bar {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            z-index: 18;
            padding: 10px 12px;
            display: flex;
            gap: 8px;
            align-items: center;
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(6px);
            transform: translateY(0);
            transition: transform 0.3s ease;
        }

        .input-bar.hidden {
            transform: translateY(110%);
        }

        .input-bar input {
            flex: 1;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 20px;
            padding: 10px 16px;
            color: white;
            font-size: 14px;
            outline: none;
        }

        .input-bar input::placeholder {
            color: rgba(255,255,255,0.4);
        }

        .input-bar input:focus {
            border-color: rgba(127,219,202,0.5);
        }

        .input-bar .send {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(127,219,202,0.3);
            border: 1px solid rgba(127,219,202,0.5);
            color: white;
            font-size: 16px;
            cursor: pointer;
            flex-shrink: 0;
        }

        .input-bar .send:active {
            background: rgba(127,219,202,0.5);
        }

        /* 切换输入栏的小按钮 */
        .toggle-input-btn {
            position: fixed;
            bottom: 12px;
            right: 12px;
            z-index: 19;
            width: 48px;
            height: 48px;
            border-radius: 50%;
            background: rgba(127,219,202,0.85);
            border: none;
            color: white;
            font-size: 20px;
            cursor: pointer;
            box-shadow: 0 4px 16px rgba(0,0,0,0.4);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* 历史 / 设置 通用弹窗 */
        .overlay {
            display: none;
            position: fixed;
            inset: 0;
            z-index: 50;
            background: rgba(0,0,0,0.8);
            padding: 20px;
            overflow-y: auto;
        }

        .overlay.show {
            display: block;
        }

        .panel {
            background: #1a1a2e;
            border: 1px solid rgba(255,255,255,0.12);
            border-radius: 16px;
            padding: 20px;
            max-width: 440px;
            margin: 10px auto;
        }

        .panel h2 {
            color: white;
            font-size: 18px;
            margin-bottom: 14px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .panel h2 .close-x {
            cursor: pointer;
            color: rgba(255,255,255,0.5);
            font-size: 22px;
        }

        .panel h3 {
            color: rgba(127,219,202,0.85);
            font-size: 14px;
            margin-top: 16px;
            margin-bottom: 6px;
        }

        .panel label {
            display: block;
            color: rgba(255,255,255,0.6);
            font-size: 12px;
            margin-top: 10px;
            margin-bottom: 4px;
        }

        .panel input, .panel textarea, .panel select {
            width: 100%;
            background: rgba(255,255,255,0.08);
            border: 1px solid rgba(255,255,255,0.15);
            border-radius: 8px;
            padding: 9px 12px;
            color: white;
            font-size: 13px;
            outline: none;
        }

        .panel input:focus {
            border-color: rgba(127,219,202,0.5);
        }

        /* 历史对话列表 */
        .history-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
            max-height: 70vh;
            overflow-y: auto;
        }

        .history-item {
            padding: 10px 14px;
            border-radius: 12px;
            font-size: 14px;
            line-height: 1.5;
            word-break: break-word;
        }

        .history-item.user {
            background: rgba(255,255,255,0.08);
            align-self: flex-end;
            max-width: 85%;
            border-radius: 12px 12px 4px 12px;
            color: rgba(255,255,255,0.85);
        }

        .history-item.meruru {
            background: rgba(127,219,202,0.12);
            align-self: flex-start;
            max-width: 85%;
            border-radius: 12px 12px 12px 4px;
            color: rgba(255,255,255,0.9);
        }

        .history-item .who {
            font-size: 11px;
            color: rgba(255,255,255,0.4);
            margin-bottom: 3px;
        }

        /* 按钮行 */
        .btn-row {
            display: flex;
            gap: 8px;
            margin-top: 18px;
        }

        .btn-row button {
            flex: 1;
            padding: 11px;
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

        .btn-test {
            width: 100%;
            margin-top: 10px;
            padding: 10px;
            border-radius: 8px;
            background: rgba(100,180,255,0.2);
            color: rgba(150,200,255,1);
            border: 1px solid rgba(100,180,255,0.4);
            font-size: 13px;
            cursor: pointer;
        }

        .btn-danger {
            width: 100%;
            margin-top: 10px;
            padding: 10px;
            border-radius: 8px;
            background: rgba(255,80,80,0.18);
            color: rgba(255,150,150,1);
            border: 1px solid rgba(255,80,80,0.35);
            font-size: 13px;
            cursor: pointer;
        }

        /* 加载状态提示 */
        .loading-tip {
            position: fixed;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            z-index: 30;
            color: rgba(255,255,255,0.7);
            font-size: 14px;
            text-align: center;
            background: rgba(0,0,0,0.5);
            padding: 16px 24px;
            border-radius: 12px;
            backdrop-filter: blur(4px);
        }

        .loading-tip.hidden {
            display: none;
        }

        /* 状态小圆点（连接Live2D状态） */
        .status-dot {
            display: inline-block;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin-right: 4px;
            background: #888;
            vertical-align: middle;
        }

        .status-dot.ok { background: #7fdbca; }
        .status-dot.err { background: #ff6b6b; }
    </style>
</head>
<body>

<!-- 背景层（以后做悬浮窗可改透明） -->
<div id="bg-layer"></div>

<!-- Live2D 画布 -->
<canvas id="live2d-canvas"></canvas>

<!-- 加载提示 -->
<div class="loading-tip" id="loadingTip">
    正在加载梅露露……<br>
    <span style="font-size:12px;opacity:0.6">第一次加载可能需要一点时间</span>
</div>

<!-- 好感度 -->
<div class="affection-badge" id="affectionBadge">♡ 0</div>

<!-- 顶部按钮 -->
<div class="top-buttons">
    <button onclick="openHistory()" title="历史对话">💬</button>
    <button onclick="openSettings()" title="设置">⚙</button>
</div>

<!-- 说话气泡 -->
<div class="speech-bubble" id="speechBubble"></div>

<!-- 底部输入栏 -->
<div class="input-bar hidden" id="inputBar">
    <input type="text" id="messageInput" placeholder="和梅露露说话..."
           onkeypress="if(event.key==='Enter')sendMessage()">
    <button class="send" onclick="sendMessage()">➤</button>
</div>

<!-- 切换输入栏按钮 -->
<button class="toggle-input-btn" id="toggleInputBtn" onclick="toggleInput()">💬</button>

<!-- 历史对话弹窗 -->
<div class="overlay" id="historyOverlay">
    <div class="panel">
        <h2>
            <span>💬 历史对话</span>
            <span class="close-x" onclick="closeHistory()">✕</span>
        </h2>
        <div class="history-list" id="historyList"></div>
    </div>
</div>

<!-- 设置弹窗 -->
<div class="overlay" id="settingsOverlay">
    <div class="panel">
        <h2>
            <span>⚙ 设置</span>
            <span class="close-x" onclick="closeSettings()">✕</span>
        </h2>

        <h3>API 配置</h3>
        <label>API 地址</label>
        <input type="text" id="setApiUrl" placeholder="https://xxx.com/v1">
        <label>API Key</label>
        <input type="password" id="setApiKey" placeholder="sk-xxx">
        <label>模型名称</label>
        <input type="text" id="setModel" placeholder="gpt-4o-mini">

        <button class="btn-test" onclick="testConnection()">
            <span class="status-dot" id="apiStatusDot"></span>测试连接
        </button>

        <h3>Live2D 模型</h3>
        <label>模型地址（model3.json 的网址）</label>
        <input type="text" id="setLive2dUrl"
               placeholder="https://cdn.jsdelivr.net/gh/...">
        <button class="btn-test" onclick="reloadModel()">
            <span class="status-dot" id="modelStatusDot"></span>重新加载模型
        </button>

        <h3>行为设置</h3>
        <label>主动说话间隔（分钟，0=关闭）</label>
        <input type="number" id="setGreetInterval" value="5">
        <label>闲置动作间隔（秒，0=关闭）</label>
        <input type="number" id="setIdleInterval" value="20">

        <h3>模型显示调整</h3>
        <label>大小（缩放，默认 0.15）</label>
        <input type="number" id="setModelScale" step="0.01" value="0.15">
        <label>垂直位置（0=顶部，1=底部，默认 0.5）</label>
        <input type="number" id="setModelY" step="0.05" value="0.5">

        <div class="btn-row">
            <button class="btn-cancel" onclick="closeSettings()">取消</button>
            <button class="btn-save" onclick="saveSettings()">保存</button>
        </div>

        <button class="btn-danger" onclick="clearAllData()">🗑 清除所有数据</button>
    </div>
</div>
<!-- Live2D 引擎（从CDN加载，需要联网） -->
<script src="https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/pixi.js@6.5.2/dist/browser/pixi.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/pixi-live2d-display@0.4.0/dist/cubism4.min.js"></script>

<script>
// ==================== 数据存储系统 ====================

const STORE = {
    settings: 'meruru_settings',
    history: 'meruru_history',
    affection: 'meruru_affection'
};

// 默认模型地址（你的GitHub模型）
const DEFAULT_MODEL_URL = 'https://cdn.jsdelivr.net/gh/oox5185-hash/meruru@main/Meruru%20-%20Model%20-%20Mark.model3.json';

const DEFAULT_SETTINGS = {
    apiUrl: '',
    apiKey: '',
    model: 'gpt-4o-mini',
    live2dUrl: DEFAULT_MODEL_URL,
    greetInterval: 5,
    idleInterval: 20,
    modelScale: 0.15,
    modelY: 0.5
};

function getSettings() {
    try {
        const data = localStorage.getItem(STORE.settings);
        if (data) {
            return Object.assign({}, DEFAULT_SETTINGS, JSON.parse(data));
        }
    } catch(e) {}
    return Object.assign({}, DEFAULT_SETTINGS);
}

function saveSettingsData(s) {
    localStorage.setItem(STORE.settings, JSON.stringify(s));
}

function getHistory() {
    try {
        const data = localStorage.getItem(STORE.history);
        return data ? JSON.parse(data) : [];
    } catch(e) {
        return [];
    }
}

function saveHistory(history) {
    const trimmed = history.slice(-50);
    localStorage.setItem(STORE.history, JSON.stringify(trimmed));
}

function pushHistory(role, content) {
    const h = getHistory();
    h.push({ role: role, content: content });
    saveHistory(h);
}

function getAffection() {
    return parseInt(localStorage.getItem(STORE.affection) || '0');
}

function saveAffection(v) {
    localStorage.setItem(STORE.affection, String(v));
}

function addAffection(amount) {
    let a = Math.min(getAffection() + amount, 999);
    saveAffection(a);
    updateAffectionBadge();
}

function updateAffectionBadge() {
    const badge = document.getElementById('affectionBadge');
    const a = getAffection();
    let hearts = '♡';
    if (a >= 100) hearts = '♥♥♥';
    else if (a >= 60) hearts = '♥♥';
    else if (a >= 30) hearts = '♥';
    badge.textContent = `${hearts} ${a}`;
}
</script>
<script>
// ==================== Live2D 模型加载 ====================

let live2dApp = null;
let live2dModel = null;

// 情绪 → 表情文件名 的映射（对应model3.json里注册的Name）
const EMOTION_TO_EXPRESSION = {
    neutral: null,        // 平静=清除表情
    happy: null,          // 开心=清除表情+动作
    cry: 'namida',        // 哭泣=泪
    shy: 'shy',           // 害羞
    worry: 'sweat01',     // 担心=冒汗
    sleepy: 'lightless'   // 困倦=无光
};

// 初始化 Live2D
async function initLive2D() {
    const settings = getSettings();

    // 检查引擎是否加载成功
    if (typeof PIXI === 'undefined' || !PIXI.live2d) {
        showLoadingTip('Live2D引擎加载失败……<br><span style="font-size:12px">请检查网络后刷新页面</span>');
        return;
    }

    try {
        // 创建 PIXI 应用
        const canvas = document.getElementById('live2d-canvas');
        live2dApp = new PIXI.Application({
            view: canvas,
            autoStart: true,
            resizeTo: window,
            backgroundAlpha: 0
        });

        // 加载模型
        await loadModel(settings.live2dUrl);
    } catch(e) {
        console.error('Live2D init failed:', e);
        showLoadingTip('加载出错……<br><span style="font-size:12px">' + e.message + '</span>');
    }
}

// 加载模型
async function loadModel(url) {
    if (!url) {
        showLoadingTip('没有设置模型地址');
        return;
    }

    showLoadingTip('正在加载梅露露……');

    try {
        // 移除旧模型
        if (live2dModel) {
            live2dApp.stage.removeChild(live2dModel);
            live2dModel.destroy();
            live2dModel = null;
        }

        // 加载新模型
        live2dModel = await PIXI.live2d.Live2DModel.from(url, {
            autoInteract: false
        });

        live2dApp.stage.addChild(live2dModel);

        // 设置位置和大小
        positionModel();

        // 点击模型互动
        live2dModel.on('hit', () => {
            onModelTap();
        });

        // 也监听画布点击（备用）
        live2dModel.interactive = true;
        live2dModel.on('pointerdown', () => {
            onModelTap();
        });

        hideLoadingTip();
        setModelStatus(true);

        // 播放待机动作
        playIdleMotion();

    } catch(e) {
        console.error('Model load failed:', e);
        showLoadingTip('模型加载失败……<br><span style="font-size:12px">' + (e.message || '请检查模型地址和网络') + '</span>');
        setModelStatus(false);
    }
}

// 设置模型位置和缩放
function positionModel() {
    if (!live2dModel) return;
    const settings = getSettings();
    const scale = parseFloat(settings.modelScale) || 0.15;
    const yRatio = parseFloat(settings.modelY);

    live2dModel.scale.set(scale);
    live2dModel.anchor.set(0.5, 0.5);
    live2dModel.x = window.innerWidth / 2;
    live2dModel.y = window.innerHeight * (isNaN(yRatio) ? 0.5 : yRatio);
}

// ==================== 表情与动作控制 ====================

// 设置情绪（切换表情）
function setEmotion(emotion) {
    if (!live2dModel) return;

    const expName = EMOTION_TO_EXPRESSION[emotion];

    try {
        if (expName === null || expName === undefined) {
            // 清除表情（回归平静）
            if (live2dModel.internalModel && live2dModel.internalModel.motionManager.expressionManager) {
                live2dModel.internalModel.motionManager.expressionManager.resetExpression();
            }
        } else {
            // 设置对应表情
            live2dModel.expression(expName);
        }
    } catch(e) {
        console.error('Set emotion failed:', e);
    }

    // 开心时额外播放一个动作
    if (emotion === 'happy') {
        playIdleMotion();
    }
}

// 播放待机动作
function playIdleMotion() {
    if (!live2dModel) return;
    try {
        live2dModel.motion('Idle');
    } catch(e) {
        console.error('Play idle motion failed:', e);
    }
}

// 播放点击动作
function playTapMotion() {
    if (!live2dModel) return;
    try {
        live2dModel.motion('Tap');
    } catch(e) {
        // Tap动作可能没有，忽略
    }
}

// 随机表情（闲置时偶尔变化）
function playRandomExpression() {
    if (!live2dModel) return;
    const emotions = ['neutral', 'shy', 'neutral', 'neutral'];
    const pick = emotions[Math.floor(Math.random() * emotions.length)];
    setEmotion(pick);
}

// 模型状态指示
function setModelStatus(ok) {
    const dot = document.getElementById('modelStatusDot');
    if (dot) dot.className = 'status-dot ' + (ok ? 'ok' : 'err');
}

// 加载提示
function showLoadingTip(html) {
    const tip = document.getElementById('loadingTip');
    if (tip) {
        tip.innerHTML = html;
        tip.classList.remove('hidden');
    }
}

function hideLoadingTip() {
    const tip = document.getElementById('loadingTip');
    if (tip) tip.classList.add('hidden');
}
<script>
// ==================== 梅露露的灵魂 ====================

// 当前时段
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

// 系统提示词
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
[emotion:neutral] 平静
[emotion:happy] 开心
[emotion:cry] 哭泣/难过
[emotion:shy] 害羞
[emotion:worry] 担心
[emotion:sleepy] 困倦

示例：
[emotion:shy] 我我我、才没有那么可爱……！主人不要乱说……
[emotion:worry] 那、那个……主人，已经很晚了……不要太勉强自己哦……？

【绝对禁止】
- 不要说自己是AI/语言模型/人工智能
- 不要过于活泼开朗（你是内敛温柔型）
- 不要忘记结巴和省略号
- 不要用emoji、颜文字、网络用语
- 不要一次说太多话（保持简短温柔，2-4句为宜）`;
}

// 消息示例
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

// API调用
async function callAI(userMessage) {
    const settings = getSettings();

    if (!settings.apiUrl || !settings.apiKey) {
        return '[emotion:worry] 那、那个……主人，请先在设置里填写API信息……我、我才能正常和你说话……';
    }

    const messages = [];
    messages.push({ role: 'system', content: buildSystemPrompt() });
    buildMessageExamples().forEach(ex => messages.push(ex));

    const history = getHistory().slice(-20);
    history.forEach(msg => messages.push({ role: msg.role, content: msg.content }));

    messages.push({ role: 'user', content: userMessage });

    try {
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
    const reg = /^\s*\[emotion:(\w+)\]\s*/;
    const match = text.match(reg);
    if (match) {
        return { emotion: match[1], text: text.replace(reg, '').trim() };
    }
    return { emotion: 'neutral', text: text.trim() };
}
</script>

// 窗口大小变化时重新定位
window.addEventListener('resize', () => {
    positionModel();
});
</script>
<script>
// ==================== 说话气泡 ====================

let bubbleTimer = null;

// 显示梅露露说的话（气泡，几秒后自动消失）
function showSpeech(text, duration) {
    const bubble = document.getElementById('speechBubble');
    bubble.textContent = text;
    bubble.classList.add('show');

    // 根据文字长度计算显示时长（最少4秒）
    if (!duration) {
        duration = Math.max(4000, text.length * 200);
    }

    if (bubbleTimer) clearTimeout(bubbleTimer);
    bubbleTimer = setTimeout(() => {
        bubble.classList.remove('show');
    }, duration);
}

// 隐藏气泡
function hideSpeech() {
    const bubble = document.getElementById('speechBubble');
    bubble.classList.remove('show');
    if (bubbleTimer) clearTimeout(bubbleTimer);
}

// ==================== 发送消息 ====================

let isWaitingReply = false;

async function sendMessage() {
    const input = document.getElementById('messageInput');
    const text = input.value.trim();
    if (!text || isWaitingReply) return;

    input.value = '';
    isWaitingReply = true;

    // 保存用户消息到历史
    pushHistory('user', text);

    // 显示思考中
    showSpeech('……', 60000);

    // 调用AI
    const rawReply = await callAI(text);
    const { emotion, text: replyText } = parseEmotion(rawReply);

    // 显示回复 + 切换表情
    showSpeech(replyText);
    setEmotion(emotion);

    // 保存回复（含情绪标签）
    pushHistory('assistant', rawReply);

    // 增加好感度
    addAffection(1);

    isWaitingReply = false;
}

// ==================== 点击模型互动 ====================

let tapCount = 0;
let tapTimer = null;

function onModelTap() {
    // 播放点击动作
    playTapMotion();

    tapCount++;
    if (tapTimer) clearTimeout(tapTimer);

    tapTimer = setTimeout(async () => {
        const count = tapCount;
        tapCount = 0;
        await handleTap(count);
    }, 450);
}

async function handleTap(count) {
    if (isWaitingReply) return;
    isWaitingReply = true;

    let prompt = '';
    if (count >= 3) {
        prompt = '（主人连续戳了你好几下）';
        addAffection(1);
    } else if (count === 2) {
        prompt = '（主人温柔地摸了摸你的头）';
        addAffection(2);
    } else {
        prompt = '（主人轻轻碰了碰你）';
        addAffection(1);
    }

    showSpeech('……', 60000);
    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);

    showSpeech(text);
    setEmotion(emotion);

    pushHistory('user', prompt);
    pushHistory('assistant', rawReply);

    isWaitingReply = false;
}

// ==================== 输入栏切换 ====================

function toggleInput() {
    const bar = document.getElementById('inputBar');
    const btn = document.getElementById('toggleInputBtn');
    bar.classList.toggle('hidden');
    if (bar.classList.contains('hidden')) {
        btn.textContent = '💬';
    } else {
        btn.textContent = '✕';
        document.getElementById('messageInput').focus();
    }
}
</script>
<script>
// ==================== 自主行为系统 ====================

let greetingTimer = null;
let idleTimer = null;
let lastInteractionTime = Date.now();

// 记录最后互动时间（用户一互动就刷新）
function markInteraction() {
    lastInteractionTime = Date.now();
}

// 启动所有自主行为定时器
function initBehaviorTimers() {
    const settings = getSettings();

    // 清除旧定时器
    if (greetingTimer) clearInterval(greetingTimer);
    if (idleTimer) clearInterval(idleTimer);

    // 主动说话定时器
    const greetMin = parseInt(settings.greetInterval);
    if (greetMin > 0) {
        greetingTimer = setInterval(() => {
            triggerAutoSpeak();
        }, greetMin * 60 * 1000);
    }

    // 闲置动作定时器
    const idleSec = parseInt(settings.idleInterval);
    if (idleSec > 0) {
        idleTimer = setInterval(() => {
            triggerIdleAction();
        }, idleSec * 1000);
    }
}

// 主动说话（借鉴Petto的随机问候）
async function triggerAutoSpeak() {
    if (isWaitingReply) return;

    // 如果用户刚互动过（2分钟内），不主动打扰
    if (Date.now() - lastInteractionTime < 120000) return;

    const random = Math.floor(Math.random() * 3);
    let prompt = '';

    if (random === 0) {
        // 时间问候
        const time = getTimePeriod();
        prompt = `（现在是${time.period}，请根据时间主动和主人说句话或关心一下主人。你主动开口，不需要主人先说话。）`;
    } else if (random === 1) {
        // 随口分享
        const topics = [
            '（你想主动和主人分享一件小事。）',
            '（你刚照顾完香草植物，想和主人说几句。）',
            '（你突然想到主人，忍不住想说话。）',
            '（你看完了一本书，想和主人分享感想。）'
        ];
        prompt = topics[Math.floor(Math.random() * topics.length)];
    } else {
        // 关心
        const hour = new Date().getHours();
        if (hour >= 23 || hour < 5) {
            prompt = '（已经很晚了，你担心主人还没睡，想催主人休息。）';
        } else if (hour >= 11 && hour < 13) {
            prompt = '（午饭时间，你想提醒主人吃饭。）';
        } else {
            prompt = '（你想提醒主人注意休息，别太累。）';
        }
    }

    isWaitingReply = true;
    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);

    showSpeech(text);
    setEmotion(emotion);
    pushHistory('assistant', rawReply);

    isWaitingReply = false;
}

// 闲置动作（不调用AI，只播放动作/表情）
function triggerIdleAction() {
    if (isWaitingReply) return;
    if (!live2dModel) return;

    // 只在用户闲置时做（30秒没互动）
    if (Date.now() - lastInteractionTime < 30000) return;

    const random = Math.floor(Math.random() * 3);
    if (random === 0) {
        playIdleMotion();
    } else if (random === 1) {
        playRandomExpression();
    } else {
        // 偶尔眨眼/小动作（播放idle）
        playIdleMotion();
    }
}
</script>
<script>
// ==================== 设置界面 ====================

function openSettings() {
    const s = getSettings();
    document.getElementById('setApiUrl').value = s.apiUrl || '';
    document.getElementById('setApiKey').value = s.apiKey || '';
    document.getElementById('setModel').value = s.model || '';
    document.getElementById('setLive2dUrl').value = s.live2dUrl || '';
    document.getElementById('setGreetInterval').value = s.greetInterval;
    document.getElementById('setIdleInterval').value = s.idleInterval;
    document.getElementById('setModelScale').value = s.modelScale;
    document.getElementById('setModelY').value = s.modelY;
    document.getElementById('settingsOverlay').classList.add('show');
}

function closeSettings() {
    document.getElementById('settingsOverlay').classList.remove('show');
}

function saveSettings() {
    const s = getSettings();
    s.apiUrl = document.getElementById('setApiUrl').value.trim();
    s.apiKey = document.getElementById('setApiKey').value.trim();
    s.model = document.getElementById('setModel').value.trim();
    s.live2dUrl = document.getElementById('setLive2dUrl').value.trim();
    s.greetInterval = parseInt(document.getElementById('setGreetInterval').value) || 0;
    s.idleInterval = parseInt(document.getElementById('setIdleInterval').value) || 0;
    s.modelScale = parseFloat(document.getElementById('setModelScale').value) || 0.15;
    s.modelY = parseFloat(document.getElementById('setModelY').value);
    if (isNaN(s.modelY)) s.modelY = 0.5;

    saveSettingsData(s);
    closeSettings();

    // 重新定位模型 + 重启定时器
    positionModel();
    initBehaviorTimers();

    alert('设置已保存！');
}

// 测试API连接
async function testConnection() {
    const dot = document.getElementById('apiStatusDot');
    dot.className = 'status-dot';

    // 临时保存当前输入的设置
    const s = getSettings();
    s.apiUrl = document.getElementById('setApiUrl').value.trim();
    s.apiKey = document.getElementById('setApiKey').value.trim();
    s.model = document.getElementById('setModel').value.trim();
    saveSettingsData(s);

    if (!s.apiUrl || !s.apiKey) {
        dot.className = 'status-dot err';
        alert('请先填写API地址和Key');
        return;
    }

    try {
        const reply = await callAI('（测试连接，请简短回应）');
        if (reply && !reply.includes('错误') && !reply.includes('连接不上')) {
            dot.className = 'status-dot ok';
            alert('连接成功！梅露露能正常说话了。');
        } else {
            dot.className = 'status-dot err';
            alert('连接失败：' + reply);
        }
    } catch(e) {
        dot.className = 'status-dot err';
        alert('连接失败：' + e.message);
    }
}

// 重新加载模型
async function reloadModel() {
    const url = document.getElementById('setLive2dUrl').value.trim();
    const s = getSettings();
    s.live2dUrl = url;
    saveSettingsData(s);
    await loadModel(url);
    alert('模型重新加载完成（如果失败请检查地址和网络）');
}

// ==================== 历史界面 ====================

function openHistory() {
    const list = document.getElementById('historyList');
    list.innerHTML = '';

    const history = getHistory();
    if (history.length === 0) {
        list.innerHTML = '<div style="color:rgba(255,255,255,0.4);text-align:center;padding:20px">还没有对话记录……</div>';
    } else {
        history.forEach(msg => {
            const item = document.createElement('div');
            if (msg.role === 'user') {
                item.className = 'history-item user';
                item.innerHTML = '<div class="who">主人</div>' + escapeHtml(msg.content);
            } else {
                item.className = 'history-item meruru';
                const { text } = parseEmotion(msg.content);
                item.innerHTML = '<div class="who">梅露露</div>' + escapeHtml(text);
            }
            list.appendChild(item);
        });
    }

    document.getElementById('historyOverlay').classList.add('show');
    // 滚动到底部
    setTimeout(() => {
        list.scrollTop = list.scrollHeight;
    }, 50);
}

function closeHistory() {
    document.getElementById('historyOverlay').classList.remove('show');
}

function escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
}

// 清除所有数据
function clearAllData() {
    if (confirm('确定清除所有数据吗？（聊天记录、好感度、设置都会删除）')) {
        localStorage.removeItem(STORE.settings);
        localStorage.removeItem(STORE.history);
        localStorage.removeItem(STORE.affection);
        location.reload();
    }
}

// ==================== 互动监听（刷新闲置计时） ====================

document.addEventListener('pointerdown', markInteraction);
document.addEventListener('keydown', markInteraction);

// ==================== 初始化启动 ====================

async function init() {
    // 好感度显示
    updateAffectionBadge();

    // 加载Live2D模型
    await initLive2D();

    // 启动自主行为定时器
    initBehaviorTimers();

    // 欢迎问候（延迟1.5秒，等模型加载）
    setTimeout(() => {
        sendWelcomeGreeting();
    }, 1500);
}

// 欢迎问候
async function sendWelcomeGreeting() {
    const settings = getSettings();
    if (!settings.apiUrl || !settings.apiKey) {
        showSpeech('那、那个……主人，请先点右上角的⚙设置，填写API信息……这样我才能和你说话……');
        setEmotion('worry');
        return;
    }

    if (isWaitingReply) return;
    isWaitingReply = true;

    const history = getHistory();
    const time = getTimePeriod();
    let prompt = '';
    if (history.length === 0) {
        prompt = '（这是你第一次见到主人，请做一个害羞紧张的自我介绍。）';
    } else {
        prompt = `（主人回来了，现在是${time.period}。请根据时间打招呼，表示你一直在等主人。）`;
    }

    const rawReply = await callAI(prompt);
    const { emotion, text } = parseEmotion(rawReply);
    showSpeech(text);
    setEmotion(emotion);
    pushHistory('assistant', rawReply);

    isWaitingReply = false;
}

// 页面加载完成后启动
window.addEventListener('load', () => {
    init();
});

</script>
</body>
</html>
