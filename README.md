<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>漢字メモ＆テスト</title>
    <style>
        :root {
            --bg-color: #f7f9fa;
            --card-bg: #ffffff;
            --text-main: #333333;
            --accent-color: #007aff;
            --border-color: #dddddd;
            --success-color: #34c759;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
        }
        h1 {
            text-align: center;
            font-size: 1.5rem;
            margin-bottom: 20px;
        }
        /* タブ切り替え */
        .tabs {
            display: flex;
            border-bottom: 2px solid var(--border-color);
            margin-bottom: 20px;
        }
        .tab {
            flex: 1;
            text-align: center;
            padding: 12px;
            cursor: pointer;
            font-weight: bold;
            color: #666;
        }
        .tab.active {
            color: var(--accent-color);
            border-bottom: 3px solid var(--accent-color);
            margin-bottom: -2px;
        }
        .panel {
            display: none;
        }
        .panel.active {
            display: block;
        }
        /* カードスタイル */
        .card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 24px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            margin-bottom: 20px;
            border: 1px solid rgba(0,0,0,0.05);
        }
        /* テスト画面 */
        .quiz-kanji {
            font-size: 3rem;
            text-align: center;
            margin: 20px 0;
            font-weight: bold;
        }
        .quiz-details {
            display: none;
            border-top: 1px dashed var(--border-color);
            padding-top: 15px;
            margin-top: 15px;
        }
        .detail-item {
            margin-bottom: 10px;
        }
        .label {
            font-size: 0.85rem;
            color: #666;
            font-weight: bold;
        }
        .btn {
            display: block;
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            cursor: pointer;
            font-weight: bold;
            margin-top: 10px;
            text-align: center;
        }
        .btn-primary { background-color: var(--accent-color); color: white; }
        .btn-success { background-color: var(--success-color); color: white; }
        .btn-secondary { background-color: #8e8e93; color: white; }
        .quiz-actions {
            display: none;
            gap: 10px;
            margin-top: 15px;
        }
        /* メモ追加フォーム */
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-size: 0.9rem;
            font-weight: bold;
        }
        .form-control {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 1rem;
        }
        /* 一覧表示 */
        .kanji-list-item {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 10px;
            cursor: pointer;
            border: 1px solid rgba(0,0,0,0.05);
        }
        .kanji-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .kanji-text {
            font-size: 1.4rem;
            font-weight: bold;
        }
        .badge {
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 0.75rem;
            font-weight: bold;
        }
        .badge-red { background: #ffe2e2; color: #ff3b30; }
        .badge-green { background: #e2f9e6; color: var(--success-color); }
        .kanji-body {
            display: none;
            margin-top: 12px;
            padding-top: 12px;
            border-top: 1px solid #eee;
        }
        .kanji-list-item.open .kanji-body {
            display: block;
        }
        .toggle-status-btn {
            background: none;
            border: 1px solid var(--border-color);
            padding: 4px 8px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.8rem;
            margin-top: 8px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>漢字メモ ＆ テスト</h1>

    <!-- タブ -->
    <div class="tabs">
        <div class="tab active" onclick="switchTab('quiz')">漢字テスト</div>
        <div class="tab" onclick="switchTab('memo')">漢字メモ一覧</div>
    </div>

    <!-- パネル1：漢字テスト -->
    <div id="quiz-panel" class="panel active">
        <div id="quiz-container" class="card">
            <!-- JavaScriptで動的に中身を入れ替える -->
        </div>
    </div>

    <!-- パネル2：漢字メモ一覧 -->
    <div id="memo-panel" class="panel">
        <!-- 追加フォーム -->
        <div class="card">
            <h3 style="margin-top:0;">新しい漢字を登録</h3>
            <form id="kanji-form" onsubmit="addKanji(event)">
                <div class="form-group">
                    <label>漢字</label>
                    <input type="text" id="form-kanji" class="form-control" required placeholder="例：稟議">
                </div>
                <div class="form-group">
                    <label>読み</label>
                    <input type="text" id="form-reading" class="form-control" required placeholder="例：りんぎ">
                </div>
                <div class="form-group">
                    <label>意味</label>
                    <textarea id="form-meaning" class="form-control" rows="2" required placeholder="例：書類を回して承認を求めること。"></textarea>
                </div>
                <div class="form-group">
                    <label>例文</label>
                    <input type="text" id="form-example" class="form-control" placeholder="例：新しいPCの購入について稟議を通す。">
                </div>
                <button type="submit" class="btn btn-primary">登録する</button>
            </form>
        </div>

        <!-- 一覧リスト -->
        <h2>登録した漢字一覧</h2>
        <div id="kanji-list"></div>
    </div>
</div>

<script>
    // サンプルデータ（LocalStorageが空のときの初期値）
    const defaultData = [
        { id: 1, kanji: "稟議", reading: "りんぎ", meaning: "会社などで案を関係者に回し、承認を求めること。", example: "稟議書を作成して上司に提出する。", isMastered: false },
        { id: 2, kanji: "補填", reading: "ほてん", meaning: "不足している部分を埋めて補うこと。", example: "赤字を自己資金で補填する。", isMastered: false }
    ];

    // LocalStorageからデータを読み込む
    let kanjiData = JSON.parse(localStorage.getItem('kanjiData')) || defaultData;
    let currentQuizIndex = 0;
    let activeQuizCards = [];

    // データを保存する関数
    function saveToStorage() {
        localStorage.setItem('kanjiData', JSON.stringify(kanjiData));
    }

    // タブ切り替え
    function switchTab(tabName) {
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));

        if(tabName === 'quiz') {
            document.querySelectorAll('.tab')[0].classList.add('active');
            document.getElementById('quiz-panel').classList.add('active');
            initQuiz();
        } else {
            document.querySelectorAll('.tab')[1].classList.add('active');
            document.getElementById('memo-panel').classList.add('active');
            renderList();
        }
    }

    // 漢字の追加
    function addKanji(e) {
        e.preventDefault();
        const newKanji = {
            id: Date.now(),
            kanji: document.getElementById('form-kanji').value,
            reading: document.getElementById('form-reading').value,
            meaning: document.getElementById('form-meaning').value,
            example: document.getElementById('form-example').value || "（例文なし）",
            isMastered: false
        };

        kanjiData.push(newKanji);
        saveToStorage();
        document.getElementById('kanji-form').reset();
        renderList();
        alert("登録しました！");
    }

    // リストの描画
    function renderList() {
        const listContainer = document.getElementById('kanji-list');
        listContainer.innerHTML = '';

        if(kanjiData.length === 0) {
            listContainer.innerHTML = '<p style="text-align:center; color:#666;">登録された漢字はありません。</p>';
            return;
        }

        // 最新の登録が上に来るように逆順で回す
        [...kanjiData].reverse().forEach(item => {
            const div = document.createElement('div');
            div.className = 'kanji-list-item';
            div.onclick = (e) => {
                // ボタンクリック時は発火させない
                if(e.target.tagName !== 'BUTTON') div.classList.toggle('open');
            };

            div.innerHTML = `
                <div class="kanji-header">
                    <span class="kanji-text">${item.kanji}</span>
                    <span class="badge ${item.isMastered ? 'badge-green' : 'badge-red'}">
                        ${item.isMastered ? '覚えた' : '未習得'}
                    </span>
                </div>
                <div class="kanji-body">
                    <div class="detail-item"><div class="label">読み</div><div>${item.reading}</div></div>
                    <div class="detail-item"><div class="label">意味</div><div>${item.meaning}</div></div>
                    <div class="detail-item"><div class="label">例文</div><div>${item.example}</div></div>
                    <button class="toggle-status-btn" onclick="toggleStatus(${item.id})">
                        ${item.isMastered ? '「未習得」に戻す' : '「覚えた」にする'}
                    </button>
                </div>
            `;
            listContainer.appendChild(div);
        });
    }

    // ステータス切り替え（一覧画面用）
    function toggleStatus(id) {
        kanjiData = kanjiData.map(item => item.id === id ? {...item, isMastered: !item.isMastered} : item);
        saveToStorage();
        renderList();
    }

    // テストの初期化
    function initQuiz() {
        // まだ覚えてない漢字だけを抽出
        activeQuizCards = kanjiData.filter(item => !item.isMastered);
        currentQuizIndex = 0;
        renderQuizCard();
    }

    // テストカードの描画
    function renderQuizCard() {
        const container = document.getElementById('quiz-container');

        if (activeQuizCards.length === 0) {
            container.innerHTML = `
                <div style="text-align: center; padding: 20px 0;">
                    <h2 style="color: var(--success-color);">全問クリア！ 🎉</h2>
                    <p>現在、覚えていない漢字はありません。素晴らしい！</p>
                    <button class="btn btn-primary" onclick="switchTab('memo')" style="width:auto; display:inline-block; padding:10px 20px;">新しい漢字を追加する</button>
                </div>
            `;
            return;
        }

        const currentCard = activeQuizCards[currentQuizIndex];
        container.innerHTML = `
            <div style="text-align:center; color:#8e8e93; font-size:0.9rem;">
                残り ${activeQuizCards.length - currentQuizIndex} 問
            </div>
            <div class="quiz-kanji">${currentCard.kanji}</div>
            
            <button id="show-answer-btn" class="btn btn-primary" onclick="showQuizAnswer()">答えを見る</button>
            
            <div id="quiz-details" class="quiz-details">
                <div class="detail-item"><div class="label">読み</div><div style="font-size:1.2rem; font-weight:bold; color:var(--accent-color);">${currentCard.reading}</div></div>
                <div class="detail-item"><div class="label">意味</div><div>${currentCard.meaning}</div></div>
                <div class="detail-item"><div class="label">例文</div><div>${currentCard.example}</div></div>
            </div>

            <div id="quiz-actions" class="quiz-actions">
                <button class="btn btn-success" onclick="submitQuizResult(true)">覚えた！</button>
                <button class="btn btn-secondary" onclick="submitQuizResult(false)">まだ覚えてない</button>
            </div>
        `;
    }

    // テストの答えを表示
    function showQuizAnswer() {
        document.getElementById('show-answer-btn').style.display = 'none';
        document.getElementById('quiz-details').style.display = 'block';
        document.getElementById('quiz-actions').style.display = 'flex';
    }

    // テスト結果の処理
    function submitQuizResult(isMastered) {
        const currentCard = activeQuizCards[currentQuizIndex];

        if (isMastered) {
            // 元のデータ構造のステータスを更新
            kanjiData = kanjiData.map(item => item.id === currentCard.id ? {...item, isMastered: true} : item);
            saveToStorage();
        }

        // 次のクイズへ（「まだ覚えてない」ならインデックスを進める。覚えた場合は配列から実質消えるのでインデックスはそのまま）
        if (!isMastered) {
            currentQuizIndex++;
        } else {
            activeQuizCards.splice(currentQuizIndex, 1);
        }

        // 最後の問題を超えたら最初からやり直す（残っている未習得のものだけ）
        if (currentQuizIndex >= activeQuizCards.length) {
            currentQuizIndex = 0;
        }

        renderQuizCard();
    }

    // 最初にページを開いたとき
    initQuiz();
</script>

</body>
</html>
