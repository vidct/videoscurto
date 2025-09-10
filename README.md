<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Vídeo Estilo TikTok</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: #000;
      font-family: sans-serif;
      color: white;
      overflow: hidden;
    }

    .container {
      width: 100vw;
      height: 100vh;
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }

    video {
      max-height: 80vh;
      max-width: 100%;
      border-radius: 10px;
      cursor: pointer;
    }

    .info {
      margin-top: 10px;
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
      margin-top: 5px;
      font-size: 16px;
      opacity: 0.8;
    }

    .controls {
      display: flex;
      gap: 20px;
      margin-top: 15px;
    }

    .controls button {
      font-size: 22px;
      background: none;
      border: none;
      color: white;
      cursor: pointer;
    }

    .liked {
      color: pink;
    }

    .audio-icon {
      font-size: 18px;
      margin-left: 5px;
    }

    .skip-btn {
      position: absolute;
      right: 20px;
      top: 20px;
      background: rgba(255, 255, 255, 0.1);
      color: white;
      border: 1px solid #555;
      padding: 8px 12px;
      border-radius: 5px;
      cursor: pointer;
      display: none;
    }

    /* Mostrar botão de pular apenas em telas grandes (PC) */
    @media (min-width: 768px) {
      .skip-btn {
        display: block;
      }
    }

  </style>
</head>
<body>

  <div class="container">
    <video id="video" src="video12.mp4" autoplay muted loop playsinline></video>

    <div class="info">
      <div class="username" id="userInfo">
        @privet
        <img src="https://cdn-icons-png.flaticon.com/512/12902/12902069.png" alt="Verificado">
      </div>
      <div class="title">🎯 Vídeo Incrível</div>
      <div class="audio">🎵 Áudio Original <span class="audio-icon" id="audioStatus">🔇</span></div>
    </div>

    <div class="controls">
      <button onclick="toggleLike(this)">👍</button>
      <button onclick="copyLink()">📤</button>
    </div>

    <button class="skip-btn" onclick="skipVideo()">⏭️ Pular</button>
  </div>

  <script>
    const video = document.getElementById('video');
    const audioStatus = document.getElementById('audioStatus');

    // Pausar/despausar ao clicar no vídeo
    video.addEventListener('click', () => {
      if (video.paused) {
        video.play();
      } else {
        video.pause();
      }
    });

    // Atualizar ícone de áudio (🔇 / 🔊)
    video.addEventListener('volumechange', () => {
      audioStatus.textContent = video.muted ? '🔇' : '🔊';
    });

    // Inicialmente mostrar como 🔇
    audioStatus.textContent = video.muted ? '🔇' : '🔊';

    // Curtir/descurtir
    function toggleLike(btn) {
      const liked = btn.classList.toggle('liked');
      btn.textContent = liked ? '💕' : '👍';
    }

    // Copiar link do vídeo
    function copyLink() {
      navigator.clipboard.writeText(video.src)
        .then(() => alert("📎 Link do vídeo copiado!"))
        .catch(() => alert("❌ Erro ao copiar link."));
    }

    // Simula pular vídeo (PC): apenas mostra alerta
    function skipVideo() {
      alert("🔁 Próximo vídeo (exemplo)");
      // Aqui você pode trocar src do vídeo se quiser
    }
  </script>

</body>
</html>
