[index.html](https://github.com/user-attachments/files/24484069/index.html)
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="theme-color" content="#000000">
  <title>???? ?????? ?????</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@400;700&display=swap" rel="stylesheet">
  <link rel="manifest" href="manifest.json">
  <style>
    body { font-family: 'Vazirmatn', sans-serif; background:#000; color:#fff; }
    .vinyl-spin { animation: spin 10s linear infinite; }
    .paused { animation-play-state: paused; }
    @keyframes spin { 100% { transform: rotate(360deg); } }
    .gold-grad { background: linear-gradient(45deg, #b8860b, #ffd700); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  </style>
  <script>
    if ("serviceWorker" in navigator) {
      window.addEventListener("load", () => {
        navigator.serviceWorker.register("/service-worker.js");
      });
    }
  </script>
</head>
<body class="h-screen flex flex-col">

  <!-- Top Bar -->
  <div class="p-6 text-center border-b border-white/10 bg-zinc-900/50">
    <h1 class="text-2xl font-bold gold-grad">???? ?????? ?????</h1>
    <p class="text-[10px] text-zinc-500 mt-1">???? ???? ?? ??????? ? ????????</p>
  </div>

  <!-- Player Visual -->
  <div class="flex-1 flex flex-col items-center justify-center p-6">
    <div id="disk" class="w-56 h-56 rounded-full border-8 border-zinc-800 shadow-[0_0_50px_rgba(184,134,11,0.2)] bg-zinc-900 flex items-center justify-center vinyl-spin paused">
      <div class="w-16 h-16 rounded-full bg-black border-2 border-yellow-600 flex items-center justify-center">
        ??
      </div>
    </div>
    <div class="mt-8 text-center">
      <h2 id="track-name" class="text-xl font-bold">????? ???...</h2>
      <p id="track-count" class="text-sm text-zinc-500 mt-2">????? ??????? ?? ?????? ????</p>
    </div>
  </div>

  <!-- Controls + File Picker -->
  <div class="p-6 space-y-6 bg-zinc-900/80 rounded-t-[40px]">
    <div class="flex justify-center gap-10 items-center">
      <button onclick="prev()" class="text-2xl text-zinc-400">?</button>
      <button onclick="toggle()" class="w-16 h-16 bg-yellow-600 rounded-full text-black text-2xl flex items-center justify-center">
        <span id="p-icon">??</span>
      </button>
      <button onclick="next()" class="text-2xl text-zinc-400">?</button>
    </div>

    <div class="flex flex-col items-center">
      <label class="w-full py-4 bg-zinc-800 border border-dashed border-yellow-600/50 rounded-2xl text-center cursor-pointer hover:bg-zinc-700 transition">
        <span class="text-sm text-yellow-500">
          ?????? ??????? ?? ????? ????
        </span>
        <input type="file" id="file-input" multiple accept="audio/*" class="hidden">
      </label>
      <p class="text-[10px] text-zinc-600 mt-2 text-center">
        ??????? ??? ?? ????? ???? ??? ???????? ? ??? ???? ?? ??????? ???? ???????.
      </p>
    </div>

    <div class="mt-4 text-center text-[10px] text-zinc-600">
      ???? ????? ???? ????? ???? ??????
    </div>
  </div>

  <audio id="main-audio"></audio>

  <script>
    const fileInput = document.getElementById('file-input');
    const audio = document.getElementById('main-audio');
    const disk = document.getElementById('disk');
    const pIcon = document.getElementById('p-icon');
    const trackName = document.getElementById('track-name');
    const trackCount = document.getElementById('track-count');

    let playlist = [];
    let currentIdx = 0;

    fileInput.onchange = (e) => {
      const files = Array.from(e.target.files);
      if (files.length > 0) {
        playlist = files;
        currentIdx = 0;
        loadTrack(0);
        trackCount.innerText = `${files.length} ???? ???????? ??`;
      }
    };

    function loadTrack(idx) {
      const file = playlist[idx];
      trackName.innerText = file.name.replace(/\.[^/.]+$/, "");
      const url = URL.createObjectURL(file);
      audio.src = url;
      audio.play();
      pIcon.textContent = "?";
      disk.classList.remove('paused');
    }

    function toggle() {
      if (playlist.length === 0) return;
      if (audio.paused) {
        audio.play();
        pIcon.textContent = "?";
        disk.classList.remove('paused');
      } else {
        audio.pause();
        pIcon.textContent = "??";
        disk.classList.add('paused');
      }
    }

    function next() {
      if (playlist.length === 0) return;
      currentIdx = (currentIdx + 1) % playlist.length;
      loadTrack(currentIdx);
    }

    function prev() {
      if (playlist.length === 0) return;
      currentIdx = (currentIdx - 1 + playlist.length) % playlist.length;
      loadTrack(currentIdx);
    }

    audio.onended = next;
  </script>
</body>
</html>
