<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>点击致富 - 闲置点击游戏</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            background-color: #2c3e50;
            color: white;
            padding: 15px 0;
            text-align: center;
            border-radius: 8px 8px 0 0;
            margin-bottom: 20px;
        }
        
        .game-title {
            margin: 0;
            font-size: 2.2em;
        }
        
        .game-version {
            font-size: 0.8em;
            opacity: 0.8;
        }
        
        .stats-panel {
            background-color: #34495e;
            color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        
        .stat {
            margin: 5px 10px;
        }
        
        .stat-value {
            font-size: 1.5em;
            font-weight: bold;
            color: #f1c40f;
        }
        
        .click-area {
            background-color: #3498db;
            height: 200px;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: transform 0.1s;
        }
        
        .click-area:active {
            transform: scale(0.98);
        }
        
        .click-area-text {
            color: white;
            font-size: 1.5em;
            text-align: center;
            user-select: none;
        }
        
        .click-effect {
            position: absolute;
            color: white;
            font-weight: bold;
            font-size: 1.2em;
            pointer-events: none;
            display: none;
            animation: floatUp 1s ease-out forwards;
        }
        
        @keyframes floatUp {
            0% {
                transform: translateY(0);
                opacity: 1;
            }
            100% {
                transform: translateY(-50px);
                opacity: 0;
            }
        }
        
        .ad-panel {
            background-color: #e74c3c;
            color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .ad-btn {
            background-color: #f1c40f;
            color: #2c3e50;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }
        
        .ad-btn:hover {
            background-color: #f39c12;
        }
        
        .ad-btn:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }
        
        .ad-btn.active-bonus {
            background-color: #27ae60;
            color: white;
        }
        
        .ad-btn.loading {
            background-color: #7f8c8d;
        }
        
        .ad-btn img {
            height: 20px;
        }
        
        .upgrades-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .upgrade {
            background-color: white;
            border-radius: 8px;
            padding: 15px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            display: flex;
            gap: 15px;
        }
        
        .upgrade img {
            width: 60px;
            height: 60px;
            object-fit: contain;
            border-radius: 5px;
        }
        
        .upgrade-info {
            flex: 1;
        }
        
        .upgrade-info h3 {
            margin: 0 0 5px 0;
            color: #2c3e50;
        }
        
        .upgrade-info p {
            margin: 3px 0;
            font-size: 0.9em;
            color: #7f8c8d;
        }
        
        .buy-btn {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 5px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            width: 100%;
            margin-top: 5px;
        }
        
        .buy-btn:hover {
            background-color: #27ae60;
        }
        
        .buy-btn:disabled {
            background-color: #bdc3c7;
            cursor: not-allowed;
        }
        
        .buy-btn.affordable {
            background-color: #3498db;
        }
        
        .buy-btn.affordable:hover {
            background-color: #2980b9;
        }
        
        .buy-btn.not-enough {
            animation: shake 0.5s;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }
        
        footer {
            text-align: center;
            padding: 15px;
            color: #7f8c8d;
            font-size: 0.8em;
        }
        
        @media (max-width: 600px) {
            .upgrades-container {
                grid-template-columns: 1fr;
            }
            
            .stats-panel {
                flex-direction: column;
                align-items: center;
                text-align: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1 class="game-title">点击致富</h1>
            <div class="game-version">v1.0.1</div>
        </header>
        
        <div class="stats-panel">
            <div class="stat">
                <div>当前资金</div>
                <div class="stat-value" id="money">0</div>
            </div>
            <div class="stat">
                <div>每秒收入</div>
                <div class="stat-value" id="income">0</div>
            </div>
        </div>
        
        <div class="click-area" id="clickArea">
            <div class="click-area-text">点击这里赚钱！</div>
            <div class="click-effect">+1</div>
        </div>
        
        <div class="ad-panel">
            <button id="rewardAdBtn" class="ad-btn">
                <img src="assets/ad-icon.png" alt="广告图标">观看广告获得2倍收益(30秒)
            </button>
        </div>
        
        <div class="upgrades-container" id="upgradesContainer">
            <!-- 资产列表将由JavaScript动态生成 -->
        </div>
        
        <footer>
            &copy; 2023 点击致富游戏 | 仅供学习使用
        </footer>
    </div>

    <script>
        // 游戏核心数据
        const gameData = {
            money: 0,
            income: 0,
            version: "1.0.1",
            upgrades: [
                { id: 1, name: "柠檬水摊", baseCost: 15, income: 0.1, owned: 0, icon: "assets/upgrade1.png" },
                { id: 2, name: "小商店", baseCost: 100, income: 1, owned: 0, icon: "assets/upgrade2.png" },
                { id: 3, name: "自动售货机", baseCost: 500, income: 4, owned: 0, icon: "assets/upgrade3.png" },
                { id: 4, name: "便利店", baseCost: 3000, income: 10, owned: 0, icon: "assets/upgrade4.png" },
                { id: 5, name: "超市", baseCost: 10000, income: 25, owned: 0, icon: "assets/upgrade5.png" },
                { id: 6, name: "购物中心", baseCost: 50000, income: 60, owned: 0, icon: "assets/upgrade6.png" },
                { id: 7, name: "商业帝国", baseCost: 250000, income: 150, owned: 0, icon: "assets/upgrade7.png" }
            ],
            adBonus: {
                active: false,
                multiplier: 2,
                endTime: 0
            }
        };

        // 初始化广告SDK - 模拟实现
        function initAdSDK() {
            console.log("广告SDK初始化完成（模拟）");
        }

        // 点击赚钱功能
        function setupClicker() {
            const clickArea = document.getElementById("clickArea");
            const clickEffect = document.querySelector(".click-effect");
            let clickSound;
            
            try {
                // 模拟点击音效
                clickSound = new Audio();
                clickSound.src = "data:audio/wav;base64,UklGRl9vT19XQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YU..."; // 简短的base64编码点击声
                clickSound.volume = 0.3;
            } catch (e) {
                console.warn("无法加载点击音效:", e);
            }
            
            clickArea.addEventListener("click", function(e) {
                // 基础收入
                let earned = 1;
                
                // 广告加成
                if(gameData.adBonus.active && Date.now() < gameData.adBonus.endTime) {
                    earned *= gameData.adBonus.multiplier;
                }
                
                gameData.money += earned;
                updateUI();
                
                // 点击效果
                if(clickSound) {
                    try {
                        clickSound.currentTime = 0;
                        clickSound.play();
                    } catch (e) {
                        console.warn("播放音效失败:", e);
                    }
                }
                
                const effect = clickEffect.cloneNode(true);
                effect.textContent = `+${earned}`;
                const rect = clickArea.getBoundingClientRect();
                effect.style.left = `${e.clientX - rect.left - 20}px`;
                effect.style.top = `${e.clientY - rect.top - 20}px`;
                effect.style.display = "block";
                effect.classList.add("click-effect-animation");
                clickArea.appendChild(effect);
                
                setTimeout(() => {
                    effect.remove();
                }, 1000);
            });
        }

        // 资产购买功能
        function setupUpgrades() {
            const container = document.getElementById("upgradesContainer");
            container.innerHTML = ""; // 清空容器
            
            gameData.upgrades.forEach(upgrade => {
                const element = document.createElement("div");
                element.className = "upgrade";
                element.innerHTML = `
                    <img src="${upgrade.icon}" alt="${upgrade.name}" onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI2MCIgaGVpZ2h0PSI2MCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiMzNDM5NGUiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIj48cGF0aCBkPSJNMjAgMTJhOCA4IDAgMSAwLTE2IDAgOCA4IDAgMCAwIDE2IDB6Ii8+PHBhdGggZD0iTTEyIDh2NCIvPjxwYXRoIGQ9Ik0xMiAxNnYuMDEiLz48L3N2Zz4='">
                    <div class="upgrade-info">
                        <h3>${upgrade.name}</h3>
                        <p>拥有: <span class="owned">${upgrade.owned}</span></p>
                        <p>收益: +<span class="income">${upgrade.income}</span>/秒</p>
                        <button class="buy-btn" data-id="${upgrade.id}">
                            购买 <span class="cost">${calculateCost(upgrade)}</span>
                        </button>
                    </div>
                `;
                container.appendChild(element);
            });
            
            // 购买事件
            document.querySelectorAll(".buy-btn").forEach(btn => {
                btn.addEventListener("click", function() {
                    const id = parseInt(this.getAttribute("data-id"));
                    buyUpgrade(id);
                });
            });
        }

        // 计算升级成本
        function calculateCost(upgrade) {
            const cost = upgrade.baseCost * Math.pow(1.15, upgrade.owned);
            return Math.min(Math.floor(cost), Number.MAX_SAFE_INTEGER);
        }

        // 购买资产
        function buyUpgrade(id) {
            const upgrade = gameData.upgrades.find(u => u.id === id);
            if(!upgrade) return;
            
            const cost = calculateCost(upgrade);
            
            if(gameData.money >= cost) {
                gameData.money -= cost;
                upgrade.owned += 1;
                gameData.income += upgrade.income;
                updateUI();
                renderUpgrades();
                
                // 每购买5次资产显示插页广告
                if(upgrade.owned % 5 === 0) {
                    setTimeout(showInterstitialAd, 1000); // 延迟1秒显示
                }
            } else {
                // 钱不够的反馈
                const btn = document.querySelector(`.buy-btn[data-id="${id}"]`);
                btn.classList.add("not-enough");
                setTimeout(() => btn.classList.remove("not-enough"), 300);
            }
        }

        // 更新UI
        function updateUI() {
            document.getElementById("money").textContent = formatNumber(gameData.money);
            document.getElementById("income").textContent = `${formatNumber(gameData.income)}/秒`;
            
            // 更新广告加成状态
            const adBtn = document.getElementById("rewardAdBtn");
            if(!adBtn) return;
            
            if(gameData.adBonus.active && Date.now() < gameData.adBonus.endTime) {
                const timeLeft = Math.ceil((gameData.adBonus.endTime - Date.now()) / 1000);
                adBtn.textContent = `广告加成中 (${timeLeft}秒)`;
                adBtn.disabled = true;
                adBtn.classList.add("active-bonus");
            } else {
                gameData.adBonus.active = false;
                adBtn.innerHTML = `<img src="assets/ad-icon.png" alt="广告图标" onerror="this.style.display='none'">观看广告获得2倍收益(30秒)`;
                adBtn.disabled = false;
                adBtn.classList.remove("active-bonus");
            }
            
            // 更新所有购买按钮状态
            renderUpgrades();
        }

        // 数字格式化
        function formatNumber(num) {
            num = Math.floor(num);
            if(num >= 1000000000) return (num/1000000000).toFixed(1) + "B";
            if(num >= 1000000) return (num/1000000).toFixed(1) + "M";
            if(num >= 1000) return (num/1000).toFixed(1) + "K";
            return num.toString();
        }

        // 渲染资产列表
        function renderUpgrades() {
            document.querySelectorAll(".upgrade").forEach((upgradeEl, index) => {
                const upgrade = gameData.upgrades[index];
                if(!upgrade) return;
                
                upgradeEl.querySelector(".owned").textContent = upgrade.owned;
                upgradeEl.querySelector(".income").textContent = upgrade.income;
                upgradeEl.querySelector(".cost").textContent = calculateCost(upgrade);
                
                // 更新按钮状态
                const btn = upgradeEl.querySelector(".buy-btn");
                if(btn) {
                    btn.disabled = gameData.money < calculateCost(upgrade);
                    btn.classList.toggle("affordable", gameData.money >= calculateCost(upgrade));
                }
            });
        }

        // 被动收入循环
        function startIncomeLoop() {
            let lastTime = Date.now();
            
            return setInterval(() => {
                const now = Date.now();
                const deltaTime = (now - lastTime) / 1000; // 转换为秒
                lastTime = now;
                
                let income = gameData.income * deltaTime;
                
                // 广告加成
                if(gameData.adBonus.active && Date.now() < gameData.adBonus.endTime) {
                    income *= gameData.adBonus.multiplier;
                }
                
                gameData.money += income;
                updateUI();
            }, 100);
        }

        // 广告功能
        function setupAds() {
            const rewardAdBtn = document.getElementById("rewardAdBtn");
            if(!rewardAdBtn) return;
            
            rewardAdBtn.addEventListener("click", function() {
                // 防止重复点击
                if(this.classList.contains("loading")) return;
                
                this.classList.add("loading");
                this.textContent = "加载广告中...";
                
                showRewardedAd(() => {
                    this.classList.remove("loading");
                });
            });
            
            // 定时显示插页广告
            return setInterval(() => {
                if(Math.random() < 0.3) { // 30%几率
                    showInterstitialAd();
                }
            }, 60000); // 每分钟检查一次
        }

        // 显示激励广告
        function showRewardedAd(onComplete) {
            // 如果已经有激活的奖励，则不处理
            if(gameData.adBonus.active && Date.now() < gameData.adBonus.endTime) {
                if(onComplete) onComplete();
                return;
            }
            
            // 模拟广告观看
            console.log("显示激励广告（模拟）");
            setTimeout(() => {
                gameData.adBonus = {
                    active: true,
                    multiplier: 2,
                    endTime: Date.now() + 30000 // 30秒
                };
                updateUI();
                if(onComplete) onComplete();
            }, 1500); // 模拟1.5秒广告观看时间
        }

        // 显示插页广告
        function showInterstitialAd() {
            console.log("显示插页广告（模拟）");
            // 在实际应用中这里会显示真正的广告
        }

        // 保存游戏
        function saveGame() {
            try {
                const saveData = {
                    money: gameData.money,
                    income: gameData.income,
                    upgrades: gameData.upgrades.map(u => ({ id: u.id, owned: u.owned })),
                    version: gameData.version
                };
                localStorage.setItem("clickToRichSave", JSON.stringify(saveData));
            } catch(e) {
                console.error("保存游戏失败:", e);
            }
        }

        // 加载游戏
        function loadGame() {
            try {
                const save = localStorage.getItem("clickToRichSave");
                if(save) {
                    const parsed = JSON.parse(save);
                    
                    // 恢复基础数据
                    gameData.money = parsed.money || 0;
                    gameData.income = parsed.income || 0;
                    
                    // 恢复升级数据
                    if(parsed.upgrades) {
                        parsed.upgrades.forEach(savedUpgrade => {
                            const upgrade = gameData.upgrades.find(u => u.id === savedUpgrade.id);
                            if(upgrade) {
                                const ownedDiff = savedUpgrade.owned - upgrade.owned;
                                upgrade.owned = savedUpgrade.owned;
                                gameData.income += upgrade.income * ownedDiff;
                            }
                        });
                    }
                    
                    console.log("游戏加载成功");
                }
            } catch(e) {
                console.error("加载游戏失败:", e);
            }
        }

        // 初始化游戏
        function initGame() {
            try {
                // 先加载游戏数据
                loadGame();
                
                // 初始化广告
                initAdSDK();
                
                // 设置游戏功能
                setupClicker();
                setupUpgrades();
                const incomeInterval = startIncomeLoop();
                const adInterval = setupAds();
                
                // 初始UI更新
                updateUI();
                
                // 自动保存
                const saveInterval = setInterval(saveGame, 10000);
                
                // 窗口关闭前保存
                window.addEventListener("beforeunload", () => {
                    saveGame();
                    clearInterval(incomeInterval);
                    clearInterval(adInterval);
                    clearInterval(saveInterval);
                });
                
                console.log("游戏初始化完成");
            } catch(e) {
                console.error("游戏初始化失败:", e);
            }
        }

        // 启动游戏
        document.addEventListener("DOMContentLoaded", initGame);
    </script>
</body>
</html># -
