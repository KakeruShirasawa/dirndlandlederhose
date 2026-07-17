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

  <!-- 撮影モード（カメラ）と 画像きせかえモード の切り替えタブ -->
  <div class="flex w-full max-w-md bg-amber-100 rounded-lg p-1 mb-3">
    <button onclick="setMode('camera')" id="mode-camera" class="flex-1 py-2 text-xs font-bold rounded-md bg-amber-600 text-white shadow-sm transition">
      📸 リアルタイム外カメラ
    </button>
    <button onclick="setMode('image')" id="mode-image" class="flex-1 py-2 text-xs font-bold rounded-md text-amber-800 transition">
      🖼️ 画像きせかえ
    </button>
  </div>

  <!-- キャンバスエリア -->
  <div class="relative bg-white border-4 border-amber-500 rounded-lg shadow-lg overflow-hidden">
    <video id="webcam" autoplay playsinline muted style="display:none;"></video>
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
      <!-- 向き・重ね順の調整タブ -->
      <button onclick="switchCategory('adjust')" id="tab-adjust" class="flex-1 py-3 text-sm font-bold text-gray-500 text-center">
        🔄 向き・重なり調整
      </button>
    </div>

    <!-- アイテム・調整選択パネル -->
    <div class="p-4 bg-gray-50">
      <!-- 1. 衣装カテゴリ（ボタンのアイコンを実際の画像に変更しました） -->
      <div id="cat-costume" class="flex space-x-3 overflow-x-auto no-scrollbar py-2">
        <button onclick="addAppImage('d-1.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-1.png" class="w-12 h-12 object-contain" alt="衣装1">
          <span class="text-[10px] text-gray-500 mt-1">衣装1</span>
        </button>
        <button onclick="addAppImage('d-2.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-2.png" class="w-12 h-12 object-contain" alt="衣装2">
          <span class="text-[10px] text-gray-500 mt-1">衣装2</span>
        </button>
        <button onclick="addAppImage('d-3.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-3.png" class="w-12 h-12 object-contain" alt="衣装3">
          <span class="text-[10px] text-gray-500 mt-1">衣装3</span>
        </button>
        <button onclick="addAppImage('d-4.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-4.png" class="w-12 h-12 object-contain" alt="衣装4">
          <span class="text-[10px] text-gray-500 mt-1">衣装4</span>
        </button>
        <button onclick="addAppImage('d-5.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-5.png" class="w-12 h-12 object-contain" alt="衣装5">
          <span class="text-[10px] text-gray-500 mt-1">衣装5</span>
        </button>
        <button onclick="addAppImage('d-6.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-6.png" class="w-12 h-12 object-contain" alt="衣装6">
          <span class="text-[10px] text-gray-500 mt-1">衣装6</span>
        </button>
        <button onclick="addAppImage('d-7.png', 200)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="d-7.png" class="w-12 h-12 object-contain" alt="衣装7">
          <span class="text-[10px] text-gray-500 mt-1">衣装7</span>
        </button>
      </div>

      <!-- 2. 小物カテゴリ（ボタンのアイコンをリボンやシューズの画像に変更しました） -->
      <div id="cat-item" class="hidden flex space-x-3 overflow-x-auto no-scrollbar py-2">
        <button onclick="addAppImage('r-1.png', 100)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="r-1.png" class="w-12 h-12 object-contain" alt="リボン">
          <span class="text-[10px] text-gray-500 mt-1">リボン</span>
        </button>
        <button onclick="addAppImage('s-1.png', 100)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="s-1.png" class="w-12 h-12 object-contain" alt="シューズ1">
          <span class="text-[10px] text-gray-500 mt-1">シューズ1</span>
        </button>
        <button onclick="addAppImage('s-a.png', 100)" class="flex-shrink-0 w-20 h-20 bg-white border-2 border-amber-200 rounded-lg flex flex-col items-center justify-center shadow-sm hover:border-amber-500 overflow-hidden">
          <img src="s-a.png" class="w-12 h-12 object-contain" alt="シューズA">
          <span class="text-[10px] text-gray-500 mt-1">シューズA</span>
        </button>
      </div>

      <!-- 3. 向き・重なり調整カテゴリ -->
      <div id="cat-adjust" class="hidden flex space-x-2 py-2">
        <button onclick="flipSelectedX()" class="flex-1 py-3 bg-white border border-gray-300 rounded-lg text-xs font-bold text-gray-700 shadow-sm hover:bg-gray-100 flex flex-col items-center justify-center">
          <span>🔄 左右反転</span>
          <span class="text-[9px] text-gray-400 mt-1">人物の向きに合わせる</span>
        </button>
        <button onclick="adjustLayer('up')" class="flex-1 py-3 bg-white border border-gray-300 rounded-lg text-xs font-bold text-gray-700 shadow-sm hover:bg-gray-100 flex flex-col items-center justify-center">
          <span>➕ 重ね順を上に</span>
          <span class="text-[9px] text-gray-400 mt-1">衣装を前に出す</span>
        </button>
        <button onclick="adjustLayer('down')" class="flex-1 py-3 bg-white border border-gray-300 rounded-lg text-xs font-bold text-gray-700 shadow-sm hover:bg-gray-100 flex flex-col items-center justify-center">
          <span>➖ 重ね順を下に</span>
          <span class="text-[9px] text-gray-400 mt-1">小物を後ろに隠す</span>
        </button>
      </div>
    </div>

    <!-- アクションボタンエリア -->
    <div class="p-4 bg-white border-t border-gray-100 space-y-3">
      <!-- 画像きせかえモードの時だけ表示される、アルバム写真アップロードボタン -->
      <div id="image-upload-area" class="hidden flex space-x-2">
        <input type="file" id="uploadPhoto" accept="image/*" class="hidden" />
        <button onclick="document.getElementById('uploadPhoto').click()" class="w-full bg-amber-50 border border-amber-300 text-amber-800 font-bold py-2 rounded-lg text-sm text-center shadow-sm">
          📸 アルバムから写真を選択
        </button>
      </div>

      <div class="flex space-x-2">
        <button onclick="deleteSelected()" class="w-full bg-red-50 border border-red-300 text-red-700 font-bold py-2 rounded-lg text-sm text-center shadow-sm">
          🗑️ 選択した衣装・小物を消去
        </button>
      </div>
      <button onclick="exportImage()" class="w-full bg-amber-600 hover:bg-amber-700 text-white font-bold py-3 rounded-lg shadow-md transition text-sm">
        💾 この姿で写真を保存する
      </button>
      <button onclick="shareToLINE()" id="shareButton" class="w-full bg-[#06C755] hover:bg-[#05b34c] text-white font-bold py-3 rounded-lg shadow-md transition text-sm hidden">
        💬 LINEの友達に送る
      </button>
    </div>
  </div>

  <script>
    const canvas = new fabric.Canvas('avatarCanvas');
    const LIFF_ID = '2010740609-R7ilUhoL';
    const videoEl = document.getElementById('webcam');
    let fabricVideo = null;
    let isStreamActive = false;
    let currentMode = 'camera';

    async function initializeLiff() {
      try {
        await liff.init({ liffId: LIFF_ID });
        if (liff.isLoggedIn()) {
          const profile = await liff.getProfile();
          document.getElementById('welcomeMessage').innerText = `${profile.displayName}さんのフェス衣装`;
          document.getElementById('shareButton').classList.remove('hidden');
          setMode('camera');
        } else {
          liff.login();
        }
      } catch (err) {
        console.error('LIFF Init failed', err);
        setMode('camera');
      }
    }

    function setMode(mode) {
      currentMode = mode;
      const camBtn = document.getElementById('mode-camera');
      const imgBtn = document.getElementById('mode-image');
      const uploadArea = document.getElementById('image-upload-area');

      if (mode === 'camera') {
        camBtn.className = "flex-1 py-2 text-xs font-bold rounded-md bg-amber-600 text-white shadow-sm transition";
        imgBtn.className = "flex-1 py-2 text-xs font-bold rounded-md text-amber-800 transition";
        uploadArea.classList.add('hidden');
        removeBackground();
        startCamera();
      } else {
        imgBtn.className = "flex-1 py-2 text-xs font-bold rounded-md bg-amber-600 text-white shadow-sm transition";
        camBtn.className = "flex-1 py-2 text-xs font-bold rounded-md text-amber-800 transition";
        uploadArea.classList.remove('hidden');
        stopCamera();
        if (liff.isLoggedIn()) {
          liff.getProfile().then(profile => {
            if (profile.pictureUrl) setBackgroundImage(profile.pictureUrl);
          });
        } else {
          canvas.setBackgroundColor('#f3f4f6', canvas.renderAll.bind(canvas));
        }
      }
    }

    async function startCamera() {
      if (isStreamActive) return;
      try {
        const stream = await navigator.mediaDevices.getUserMedia({
          video: { facingMode: { ideal: "environment" }, width: 720, height: 960 },
          audio: false
        });
        videoEl.srcObject = stream;
        videoEl.play();
        isStreamActive = true;

        videoEl.addEventListener('loadedmetadata', function() {
          removeBackground();

          fabricVideo = new fabric.Image(videoEl, {
            left: 0, top: 0,
            selectable: false, hoverCursor: 'default'
          });

          const scale = Math.max(canvas.width / videoEl.videoWidth, canvas.height / videoEl.videoHeight);
          fabricVideo.set({
            scaleX: scale, scaleY: scale,
            left: (canvas.width - videoEl.videoWidth * scale) / 2,
            top: (canvas.height - videoEl.videoHeight * scale) / 2,
            isBackgroundImage: true
          });

          canvas.add(fabricVideo).sendToBack(fabricVideo);

          function updateVideoLoop() {
            if (!isStreamActive) return;
            canvas.renderAll();
            fabric.util.requestAnimFrame(updateVideoLoop);
          }
          fabric.util.requestAnimFrame(updateVideoLoop);
        });
      } catch (err) {
        console.error("カメラの起動に失敗しました:", err);
        alert("カメラの起動に失敗しました。カメラの利用権限を許可してください。");
      }
    }

    function stopCamera() {
      if (videoEl.srcObject) {
        videoEl.srcObject.getTracks().forEach(track => track.stop());
        videoEl.srcObject = null;
      }
      isStreamActive = false;
      removeBackground();
    }

    function removeBackground() {
      canvas.getObjects().forEach(obj => {
        if (obj.isBackgroundImage) canvas.remove(obj);
      });
      canvas.renderAll();
    }

    function setBackgroundImage(imageUrl) {
      removeBackground();
      fabric.Image.fromURL(imageUrl, function (img) {
        const scale = Math.min(canvas.width / img.width, canvas.height / img.height);
        img.set({
          scaleX: scale, scaleY: scale,
          left: (canvas.width - img.width * scale) / 2,
          top: (canvas.height - img.height * scale) / 2,
          selectable: false, hoverCursor: 'default',
          isBackgroundImage: true
        });
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

    function switchCategory(category) {
      ['costume', 'item', 'adjust'].forEach(cat => {
        const tab = document.getElementById(`tab-${cat}`);
        const list = document.getElementById(`cat-${cat}`);
        if (cat === category) {
          tab.classList.add('text-amber-800', 'border-b-2', 'border-amber-600');
          tab.classList.remove('text-gray-500');
          list.classList.remove('hidden'); if(cat !== 'adjust') list.classList.add('flex');
        } else {
          tab.classList.remove('text-amber-800', 'border-b-2', 'border-amber-600');
          tab.classList.add('text-gray-500');
          list.classList.add('hidden'); if(cat !== 'adjust') list.classList.remove('flex');
        }
      });
    }

    function addAppImage(fileName, initialWidth) {
      const imageUrl = `https://kakerushirasawa.github.io/dirndlandlederhose/${fileName}`;
      
      fabric.Image.fromURL(imageUrl, function (img) {
        const filter = new fabric.Image.filters.RemoveColor({
          color: '#FF00FF',
          distance: 0.15
        });
        img.filters.push(filter);
        img.applyFilters();
        
        img.scaleToWidth(initialWidth);
        img.set({
          left: 80, top: 100,
          cornerColor: '#d97706', cornerSize: 12, transparentCorners: false
        });
        
        canvas.add(img).setActiveObject(img).renderAll();
      }, { crossOrigin: 'anonymous' });
    }

    function flipSelectedX() {
      const activeObject = canvas.getActiveObject();
      if (activeObject && !activeObject.isBackgroundImage) {
        activeObject.set('flipX', !activeObject.flipX);
        canvas.renderAll();
      }
    }

    // 重ね順の調整
    function adjustLayer(direction) {
      const activeObject = canvas.getActiveObject();
      if (!activeObject || activeObject.isBackgroundImage) return;

      if (direction === 'up') {
        canvas.bringForward(activeObject);
      } else if (direction === 'down') {
        canvas.sendBackwards(activeObject);
        const bg = canvas.getObjects().find(obj => obj.isBackgroundImage);
        if (bg) {
          canvas.sendToBack(bg);
        }
      }
      canvas.renderAll();
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
      link.download = 'oktoberfest-app.png';
      link.href = dataURL;
      link.click();
    }

    async function shareToLINE() {
      if (!liff.isApiAvailable('shareTargetPicker')) return alert("利用できません");
      try {
        await liff.shareTargetPicker([{
          type: "text",
          text: "オクトーバーフェストの着せ替えカメラアプリで遊んだよ！🍻\nLINEからすぐ遊べるよ！\nhttps://liff.line.me/" + LIFF_ID
        }]);
      } catch (error) { console.error(error); }
    }

    window.onload = initializeLiff;
  </script>
</body>
</html>
