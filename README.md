<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>オクトーバーフェスト・アバターメーカー</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/5.3.1/fabric.min.js"></script>
  <script charset="utf-8" src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
  <style>
    body { background-color: #fdf6e2; }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="flex flex-col items-center justify-between min-h-screen p-4 pb-6">

  <header class="text-center my-2">
    <h1 class="text-2xl font-bold text-amber-800">🍻 Oktoberfest Maker 🍻</h1>
    <p class="text-xs text-amber-700" id="welcomeMessage">自分だけのフェス衣装を作ろう！</p>
  </header>

  <!-- キャンバスエリア -->
  <div class="relative bg-white border-4 border-amber-500 rounded-lg shadow-lg overflow-hidden">
    <canvas id="avatarCanvas" width="350" height="420"></canvas>
  </div>

  <!-- 操作パネル -->
  <div class="w-full max-w-md bg-white rounded-xl shadow-md overflow-hidden mt-4">
    
    <!-- カテゴリ選択タブ -->
    <div class="flex border-b border-gray-200 bg-amber-50">
      <button onclick="switchCategory('costume')" id="tab-costume" class="flex-1 py-3 text-sm font-bold text-amber-800 border-b-2 border-amber-600 text-center">
        👗 衣装
      </button>
      <button onclick="switchCategory('item')" id="tab-item" class="flex-1 py-3 text-sm font-bold text-gray-500 text-center">
        🎀 小物
      </button>
    </div>

    <!-- アイテム選択パネル -->
    <div class="p-4 bg-gray-50">
      <!-- 1. 衣装カテゴリ (d-1.png 〜 d-7.png に変更) -->
      <div id="cat-costume" class="flex space-x-3 overflow-x-auto no-scrollbar py-2">
        <button onclick="addAppImage('d-1.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装1</span>
        </button>
        <button onclick="addAppImage('d-2.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装2</span>
        </button>
        <button onclick="addAppImage('d-3.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装3</span>
        </button>
        <button onclick="addAppImage('d-4.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装4</span>
        </button>
        <button onclick="addAppImage('d-5.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装5</span>
        </button>
        <button onclick="addAppImage('d-6.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装6</span>
        </button>
        <button onclick="addAppImage('d-7.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👗</span><span class="text-[10px] text-gray-500 mt-1">衣装7</span>
        </button>
      </div>

      <!-- 2. 小物カテゴリ (r-1.png, s-1.png, s-a.png に変更) -->
      <div id="cat-item" class="hidden flex space-x-3 overflow-x-auto no-scrollbar py-2">
        <button onclick="addAppImage('r-1.png', 100)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">🎀</span><span class="text-[10px] text-gray-500 mt-1">リボン</span>
        </button>
        <button onclick="addAppImage('s-1.png', 100)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👞</span><span class="text-[10px] text-gray-500 mt-1">シューズ1</span>
        </button>
        <button onclick="addAppImage('s-a.png', 100)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500">
          <span class="text-2xl">👞</span><span class="text-[10px] text-gray-500 mt-1">シューズA</span>
        </button>
      </div>
    </div>

    <!-- アクションボタンエリア -->
    <div class="p-4 bg-white border-t border-gray-100 space-y-3">
      <div class="flex space-x-2">
        <input type="file" id="uploadPhoto" accept="image/*" class="hidden" />
        <button onclick="document.getElementById('uploadPhoto').click()" class="flex-1 bg-amber-50 border border-amber-300 text-amber-800 font-bold py-2 rounded-lg text-sm text-center shadow-sm">
          📸 写真変更
        </button>
        <button onclick="deleteSelected()" class="flex-1 bg-red-50 border border-red-300 text-red-700 font-bold py-2 rounded-lg text-sm text-center shadow-sm">
          🗑️ 選択消去
        </button>
      </div>
      <button onclick="exportImage()" class="w-full bg-amber-600 hover:bg-amber-700 text-white font-bold py-3 rounded-lg shadow-md transition text-sm">
        💾 端末に画像を保存する
      </button>
      <button onclick="shareToLINE()" id="shareButton" class="w-full bg-[#06C755] hover:bg-[#05b34c] text-white font-bold py-3 rounded-lg shadow-md transition text-sm hidden">
        💬 LINEの友達に送る
      </button>
    </div>
  </div>

  <script>
    const canvas = new fabric.Canvas('avatarCanvas');
    canvas.setBackgroundColor('#f3f4f6', canvas.renderAll.bind(canvas));

    const LIFF_ID = '2010740609-R7ilUhoL';

    async function initializeLiff() {
      try {
        await liff.init({ liffId: LIFF_ID });
        if (liff.isLoggedIn()) {
          const profile = await liff.getProfile();
          document.getElementById('welcomeMessage').innerText = `${profile.displayName}さんのフェス衣装`;
          document.getElementById('shareButton').classList.remove('hidden');
          if (profile.pictureUrl) setBackgroundImage(profile.pictureUrl);
        } else {
          liff.login();
        }
      } catch (err) {
        console.error('LIFF Init failed', err);
      }
    }

    function switchCategory(category) {
      ['costume', 'item'].forEach(cat => {
        const tab = document.getElementById(`tab-${cat}`);
        const list = document.getElementById(`cat-${cat}`);
        if (cat === category) {
          tab.classList.add('text-amber-800', 'border-b-2', 'border-amber-600');
          tab.classList.remove('text-gray-500');
          list.classList.remove('hidden');
          list.classList.add('flex');
        } else {
          tab.classList.remove('text-amber-800', 'border-b-2', 'border-amber-600');
          tab.classList.add('text-gray-500');
          list.classList.add('hidden');
          list.classList.remove('flex');
        }
      });
    }

    function setBackgroundImage(imageUrl) {
      fabric.Image.fromURL(imageUrl, function (img) {
        const scale = Math.min(canvas.width / img.width, canvas.height / img.height);
        img.set({
          scaleX: scale, scaleY: scale,
          left: (canvas.width - img.width * scale) / 2,
          top: (canvas.height - img.height * scale) / 2,
          selectable: false, hoverCursor: 'default'
        });
        canvas.getObjects().forEach(obj => { if (obj.isBackgroundImage) canvas.remove(obj); });
        img.isBackgroundImage = true;
        canvas.add(img).sendToBack(img).renderAll();
      }, { crossOrigin: 'anonymous' });
    }

    document.getElementById('uploadPhoto').addEventListener('change', function (e) {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = function (f) { setBackgroundImage(f.target.result); };
      reader.readAsDataURL(file);
    });

    // 画像を読み込んでピンク背景を透明にする関数
    function addAppImage(fileName, initialWidth) {
      const imageUrl = `https://kakerushirasawa.github.io/dirndlandlederhose/${fileName}`;
      
      fabric.Image.fromURL(imageUrl, function (img) {
        // 鮮やかなピンク色（#FF00FF）を透明にするフィルター
        const filter = new fabric.Image.filters.RemoveColor({
          color: '#FF00FF',
          distance: 0.15
        });
        
        img.filters.push(filter);
        img.applyFilters();
        
        img.scaleToWidth(initialWidth);
        img.set({
          left: 80,
          top: 100,
          cornerColor: '#d97706',
          cornerSize: 12,
          transparentCorners: false
        });
        
        canvas.add(img).setActiveObject(img).renderAll();
      }, { crossOrigin: 'anonymous' });
    }

    function deleteSelected() {
      const activeObject = canvas.getActiveObject();
      if (activeObject && !activeObject.isBackgroundImage) {
        canvas.remove(activeObject).renderAll();
      }
    }

    function exportImage() {
      canvas.discardActiveObject().renderAll();
      const dataURL = canvas.toDataURL({ format: 'png', quality: 1.0 });
      const link = document.createElement('a');
      link.download = 'oktoberfest-avatar.png';
      link.href = dataURL;
      link.click();
    }

    async function shareToLINE() {
      if (!liff.isApiAvailable('shareTargetPicker')) return alert("利用できません");
      try {
        await liff.shareTargetPicker([{
          type: "text",
          text: "オクトーバーフェストの民族衣装アバターを作ったよ！🍻\nLINEからすぐ遊べるよ！\nhttps://liff.line.me/" + LIFF_ID
        }]);
      } catch (error) { console.error(error); }
    }

    window.onload = initializeLiff;
  </script>
</body>
</html>
