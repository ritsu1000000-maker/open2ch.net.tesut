# open2ch.net.tesut
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>おーぷん風掲示板</title>
    <style>
        /* 基本はレンガ背景（トップページ用） */
        body {
            background-color: #d2b48c; 
            color: #000000; 
            font-family: "MS P Gothic", "Mona", "ヒラギノ角ゴ Pro W3", sans-serif;
            font-size: 16px; 
            line-height: 1.4;
            margin: 0;
            padding: 10px;
        }

        /* スレ表示モードは背景を完全な白にする */
        body.thread-mode {
            background-color: #ffffff;
            padding: 20px;
        }

        /* 画面中央に収めるコンテナ */
        .wrapper {
            max-width: 1000px;
            margin: 0 auto;
        }

        /* トップページ用：ミントグリーンの外枠ボックス */
        .bbs-box {
            background-color: #bbf2bb; 
            border: 1px solid #55cc55; 
            border-radius: 3px;
            padding: 10px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }

        /* 板タイトル＆検索バー */
        .board-top-info {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
        }
        .board-title { font-weight: bold; font-size: 16px; }
        .search-bar { text-align: right; font-size: 12px; }
        .search-bar input[type="text"] { border: 1px solid #777; padding: 2px; font-size: 12px; width: 120px; }
        
        .btn-back-home {
            display: inline-block;
            background-color: #ffffff;
            color: #000000 !important;
            border: 1px solid #55cc55;
            padding: 2px 8px;
            border-radius: 3px;
            text-decoration: none !important;
            font-weight: bold;
            margin-right: 10px;
            cursor: pointer;
        }
        .btn-back-home:hover { background-color: #e2fbe2; }

        /* スレッド一覧のリンク */
        .sidebar h2 {
            font-size: 15px;
            margin: 0 0 10px 0;
            border-bottom: 1px solid #55cc55;
            padding-bottom: 3px;
        }
        .sidebar ul { list-style: none; padding: 0; margin: 0; }
        .sidebar li { margin-bottom: 6px; font-size: 14px; }
        
        .thread-link {
            color: #0000ee;
            text-decoration: underline;
            cursor: pointer;
        }
        .thread-link:hover { color: #ff0000; }

        /* スレ表示用のデザイン */
        .thread-title { color: #000000; font-size: 18px; font-weight: normal; margin-bottom: 25px; }

        /* レスの見た目 */
        .post { margin-bottom: 18px; font-size: 16px; scroll-margin-top: 20px; }
        
        .post-num-link {
            color: #0000ee;
            text-decoration: none;
            font-weight: bold;
            cursor: pointer;
        }
        .post-num-link:hover { text-decoration: underline; }
        
        .post-name { color: #228b22; font-weight: bold; } 
        .post-name-mail { color: #0000ee; font-weight: bold; text-decoration: underline; cursor: pointer; }
        .post-body { margin-top: 5px; margin-left: 25px; font-size: 16px; color: #000000; white-space: pre-wrap; word-break: break-all; }
        
        .anchor-link { color: #0000ee; text-decoration: underline; }
        .anchor-link:hover { color: #ff0000; }

        /* ナビゲーションリンク */
        .nav-links { margin-top: 30px; font-size: 14px; border-top: 1px solid #ccc; padding-top: 10px; }
        .nav-links a { cursor: pointer; color: #0000ee; text-decoration: underline; }

        /* 書き込みフォームのスタイル */
        .post-form-box {
            background-color: #efefef;
            border: 1px solid #ccc;
            padding: 15px;
            margin-top: 20px;
            max-width: 600px;
        }
        .form-row { margin-bottom: 10px; }
        .form-row label { display: inline-block; width: 90px; font-size: 14px; }
        .form-row input[type="text"] { width: 180px; padding: 3px; margin-right: 10px; }
        .form-row input.wide-input { width: 70%; }
        .form-row textarea { width: 100%; height: 100px; padding: 5px; box-sizing: border-box; }

        /* 書き込み完了画面用のスタイル */
        .complete-screen {
            background-color: #ffffff;
            font-family: sans-serif;
            padding: 50px 20px;
            text-align: left;
        }

        .hidden { display: none !important; }
    </style>
</head>
<body id="main-body">

<div class="wrapper">

    <!-- ==================== 1. トップページ用の画面 ==================== -->
    <div id="view-top">
        <div class="bbs-box board-top-info">
            <div class="board-title">おーぷん風テスト板</div>
            <div class="search-bar">
                <input type="text" placeholder="スレッド検索">
                <button>検索</button>
            </div>
        </div>

        <div class="bbs-box info-box">
            自治からのお知らせ：URLハッシュの検知処理を強化しました。
        </div>

        <!-- スレッド一覧 -->
        <div class="bbs-box main-container">
            <div class="sidebar">
                <h2>スレッド一覧</h2>
                <ul id="thread-list"></ul>
            </div>
        </div>

        <!-- スレッド作成フォーム -->
        <div class="post-form-box">
            <h3>新規スレッド作成</h3>
            <div class="form-row">
                <label for="new-thread-title">スレタイトル：</label>
                <input type="text" id="new-thread-title" class="wide-input" placeholder="スレッドのタイトルを入力">
            </div>
            <div class="form-row">
                <label for="new-thread-name">名前：</label>
                <input type="text" id="new-thread-name" value="名無しさん＠おーぷん">
                <label for="new-thread-mail">E-mail：</label>
                <input type="text" id="new-thread-mail" value="sage">
            </div>
            <div class="form-row">
                <textarea id="new-thread-message" placeholder="1番目のレス内容を入力してください"></textarea>
            </div>
            <div class="form-row">
                <button type="button" onclick="createNewThread()">新規スレッドを立てる</button>
            </div>
        </div>
    </div>


    <!-- ==================== 2. スレッド表示用の画面 ==================== -->
    <div id="view-thread" class="hidden">
        <div>
            <a class="btn-back-home" onclick="switchToTopMode()">■掲示板トップに戻る</a>
            
            <h1 class="thread-title" id="active-thread-title">スレッドタイトル</h1>

            <!-- レス一覧エリア -->
            <div id="post-list"></div>
        </div>

        <!-- レス書き込みフォーム -->
        <div class="post-form-box">
            <h3>レスを投稿する</h3>
            <div class="form-row">
                <label for="form-name">名前：</label>
                <input type="text" id="form-name" value="名無しさん＠おーぷん">
                <label for="form-mail">E-mail：</label>
                <input type="text" id="form-mail" value="sage">
            </div>
            <div class="form-row">
                <textarea id="form-message" placeholder="ここに書き込み内容を入力してください"></textarea>
            </div>
            <div class="form-row">
                <button type="button" onclick="submitPost()">書き込む</button>
            </div>
        </div>

        <div class="nav-links">
            <a onclick="switchToTopMode()">掲示板トップに戻る</a>
        </div>
    </div>


    <!-- ==================== 3. 書き込み完了画面 ==================== -->
    <div id="view-complete" class="complete-screen hidden">
        <h2>書き込みました</h2>
        <div>
            書き込みが終わりました。<br><br>
            画面が切り替わるまでしばらくお待ちください。<br><br>
            <a id="redirect-link" style="color: #0000ee; text-decoration: underline; cursor: pointer;">待ちきれない方はこちらをクリックしてください。</a>
        </div>
    </div>

</div>

<script>
    const defaultData = [
        {
            id: 0,
            title: "おーぷん風掲示板を作ってみるスレ",
            posts: [
                {
                    num: 1,
                    name: "名無しさん＠おーぷん",
                    mail: "",
                    date: "2026/05/18(月) 11:00:00",
                    idStr: "AbCdEfGh",
                    body: "HTMLとCSSだけで掲示板の見た目を作ってみた。\nURLに#thread-IDが付くようになったのでリロードも安心だぞ。"
                }
            ]
        }
    ];

    let threadsData = [];
    let activeThreadId = null;

    // 画面読み込み時の処理を厳密化
    window.onload = function() {
        // 1. まずLocalStorageからデータを最優先で復元
        const savedData = localStorage.getItem('bbs_data');
        if (savedData) {
            threadsData = JSON.parse(savedData);
        } else {
            threadsData = defaultData;
            saveToStorage();
        }
        
        // 2. スレ一覧を一度組み立てる
        renderThreadList();

        // 3. ブラウザがURLの準備を終えるのをミリ秒単位で少し待ってからハッシュを確認
        setTimeout(checkUrlHash, 30);
    };

    // URLのハッシュ（#thread-数字）を解析して直接スレを開く関数
    function checkUrlHash() {
        const hash = window.location.hash;
        if (hash && hash.startsWith('#thread-')) {
            const threadId = parseInt(hash.replace('#thread-', ''), 10);
            const thread = threadsData.find(t => t.id === threadId);
            if (thread) {
                // すでにハッシュがあるのでURLの書き換え(updateUrl)はfalseで開く
                openThread(threadId, false);
                return;
            }
        }
        // ハッシュがない、または該当スレがないならトップへ
        switchToTopMode(false);
    }

    function saveToStorage() {
        localStorage.setItem('bbs_data', JSON.stringify(threadsData));
    }

    function renderThreadList() {
        const listContainer = document.getElementById('thread-list');
        listContainer.innerHTML = '';
        
        threadsData.forEach(thread => {
            const li = document.createElement('li');
            li.innerHTML = `<a class="thread-link" onclick="openThread(${thread.id})">${thread.id + 1}: ${thread.title} (${thread.posts.length})</a>`;
            listContainer.appendChild(li);
        });
    }

    function showCompleteScreen(nextAction) {
        document.getElementById('view-top').classList.add('hidden');
        document.getElementById('view-thread').classList.add('hidden');
        
        const body = document.getElementById('main-body');
        const originalClass = body.className;
        body.className = ''; 

        const completeView = document.getElementById('view-complete');
        completeView.classList.remove('hidden');

        let timer = setTimeout(proceed, 1500);

        document.getElementById('redirect-link').onclick = function() {
            clearTimeout(timer);
            proceed();
        };

        function proceed() {
            completeView.classList.add('hidden');
            body.className = originalClass; 
            nextAction(); 
        }
    }

    function createNewThread() {
        const titleInput = document.getElementById('new-thread-title').value.trim();
        const nameInput = document.getElementById('new-thread-name').value || '名無しさん＠おーぷん';
        const mailInput = document.getElementById('new-thread-mail').value.trim();
        const messageInput = document.getElementById('new-thread-message').value;

        if (!titleInput) { alert('スレッドタイトルを入力してください'); return; }
        if (!messageInput.trim()) { alert('1番目の本文を入力してください'); return; }

        const newThreadId = threadsData.length;
        const newThread = {
            id: newThreadId,
            title: titleInput,
            posts: [
                {
                    num: 1,
                    name: nameInput,
                    mail: mailInput,
                    date: generateDateString(),
                    idStr: generateRandomId(),
                    body: messageInput
                }
            ]
        };

        threadsData.push(newThread);
        saveToStorage();

        document.getElementById('new-thread-title').value = '';
        document.getElementById('new-thread-message').value = '';

        renderThreadList();

        showCompleteScreen(() => {
            openThread(newThreadId);
        });
    }

    // スレッドを開く
    function openThread(threadId, updateUrl = true) {
        activeThreadId = threadId;
        const thread = threadsData.find(t => t.id === threadId);
        if (!thread) return;

        if (updateUrl) {
            // アドレスバーにハッシュを乗せる
            window.location.hash = `thread-${threadId}`;
        }

        document.getElementById('active-thread-title').innerText = thread.title;
        renderPosts(thread.posts);

        document.getElementById('view-top').classList.add('hidden');
        document.getElementById('view-thread').classList.remove('hidden');
        document.getElementById('main-body').classList.add('thread-mode');
        window.scrollTo(0, 0);
    }

    function renderPosts(posts) {
        const postListContainer = document.getElementById('post-list');
        postListContainer.innerHTML = '';

        posts.forEach(post => {
            const linkedBody = convertTextToLink(post.body);
            let nameHtml = `<span class="post-name">${post.name}</span>`;
            if (post.mail) {
                nameHtml = `<a href="mailto:${post.mail}" class="post-name-mail">${post.name}</a>`;
            }

            const postHtml = `
                <div class="post" id="p${post.num}">
                    <div class="post-header">
                        <a class="post-num-link" onclick="insertAnchor(${post.num})">${post.num}</a> ：${nameHtml}：${post.date} ID:${post.idStr}
                    </div>
                    <div class="post-body">${linkedBody}</div>
                </div>
            `;
            postListContainer.insertAdjacentHTML('beforeend', postHtml);
        });
    }

    function submitPost() {
        const nameInput = document.getElementById('form-name').value || '名無しさん＠おーぷん';
        const mailInput = document.getElementById('form-mail').value.trim();
        const messageInput = document.getElementById('form-message').value;

        if (!messageInput.trim()) { alert('本文を入力してください'); return; }

        const thread = threadsData.find(t => t.id === activeThreadId);
        if (!thread) return;

        const newPostNum = thread.posts.length + 1;
        const newPost = {
            num: newPostNum,
            name: nameInput,
            mail: mailInput,
            date: generateDateString(),
            idStr: generateRandomId(),
            body: messageInput
        };

        thread.posts.push(newPost);
        saveToStorage();

        document.getElementById('form-message').value = '';

        showCompleteScreen(() => {
            openThread(activeThreadId, false); 
            window.scrollTo(0, document.body.scrollHeight);
        });
    }

    // ホームに戻る
    function switchToTopMode(clearHash = true) {
        if (clearHash) {
            // 明示的にトップへ戻るときのみハッシュを消す
            window.location.hash = '';
        }
        document.getElementById('view-thread').classList.add('hidden');
        document.getElementById('view-top').classList.remove('hidden');
        document.getElementById('main-body').classList.remove('thread-mode');
        renderThreadList();
    }

    function insertAnchor(num) {
        const textarea = document.getElementById('form-message');
        textarea.value += `>>${num}\n`;
        textarea.focus();
    }

    function convertTextToLink(text) {
        let escaped = text.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        return escaped.replace(/&gt;&gt;(\d+)/g, function(match, num) {
            return `<a href="#p${num}" class="anchor-link">${match}</a>`;
        });
    }

    function generateDateString() {
        const now = new Date();
        const days = ['日', '月', '火', '水', '木', '金', '土'];
        return `${now.getFullYear()}/${String(now.getMonth()+1).padStart(2,'0')}/${String(now.getDate()).padStart(2,'0')}(${days[now.getDay()]}) ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}:${String(now.getSeconds()).padStart(2,'0')}`;
    }

    function generateRandomId() {
        return Math.random().toString(36).substring(2, 10).toUpperCase();
    }
</script>

</body>
</html>
