<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Vídeo TikTok Custom</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: #000;
      color: white;
      font-family: sans-serif;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }

    .container {
      position: relative;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }

    video {
      max-height: 70vh;
      max-width: 100%;
      border-radius: 10px;
      cursor: pointer;
    }

    .info {
      margin-top: 12px;
      text-align: center;
    }

    .username {
      font-weight: bold;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 5px;
    }

    .username img {
      width: 16px;
      height: 16px;
    }

    .title, .audio {
      margin-top: 4px;
      font-size: 16px;
      opacity: 0.8;
    }

    .controls-left, .controls-right {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      gap: 20px;
    }

    .controls-left {
      left: 20px;
    }

    .controls-right {
      right: 20px;
    }

    button {
      font-size: 22px;
      background: none;
      border: none;
      color: white;
      cursor: pointer;
    }

    .liked {
      color: pink;
    }

    @media (max-width: 768px) {
      video {
        max-height: 60vh;
      }
    }
  </style>
</head>
<body>

  <div class="container">
    <video id="video" src="video12.mp4" autoplay muted loop playsinline></video>

    <div class="info">
      <div class="username">
        @privet
        <img src="https://cdn-icons-png.flaticon.com/512/12902/12902069.png" alt="Verificado">
      </div>
      <div class="title">🎯 Vídeo Incrível</div>
      <div class="audio">🎵 Áudio Original</div>
    </div>

    <div class="controls-left">
      <button id="muteBtn" onclick="toggleMute()">🔇 Mute</button>
    </div>

    <div class="controls-right">
      <button onclick="toggleLike(this)">👍</button>
      <button id="shareBtn" onclick="copyLink()">📤</button>
    </div>
  </div>

  <script>
    const video = document.getElementById('video');
    const muteBtn = document.getElementById('muteBtn');
    const shareBtn = document.getElementById('shareBtn');

    // 🔇 Toggle mute
    function toggleMute() {
      video.muted = !video.muted;
      muteBtn.textContent = video.muted ? '🔇 Mute' : '🔊 Desmute';
    }

    // ❤️ Toggle like
    function toggleLike(btn) {
      const liked = btn.classList.toggle('liked');
      btn.textContent = liked ? '💕' : '👍';
    }

    // 📤 Copiar link (compartilhar)
    function copyLink() {
      navigator.clipboard.writeText(video.src)
        .then(() => alert("📎 Link do vídeo copiado!"))
        .catch(() => alert("❌ Erro ao copiar link."));
    }

    // ✅ Ativar ou desativar botão de compartilhar
    const showShare = true; // coloque false para esconder
    if (!showShare) {
      shareBtn.style.display = 'none';
    }
  </script>

</body>
</html>
