<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Feed TikTok Custom</title>
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
      overflow: hidden;
    }

    .container {
      scroll-snap-type: y mandatory;
      overflow-y: scroll;
      height: 100vh;
    }

    .video-container {
      position: relative;
      width: 100vw;
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      scroll-snap-align: start;
    }

    video {
      width: 90vw;
      height: 80vh;
      object-fit: contain;
      border-radius: 10px;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
      cursor: pointer;
      background: #000;
    }

    .info {
      position: absolute;
      bottom: 80px;
      left: 20px;
      z-index: 2;
    }

    .username {
      font-weight: bold;
      display: flex;
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
      opacity: 0.9;
    }

    .controls {
      position: absolute;
      right: 20px;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      gap: 20px;
      z-index: 2;
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

    @media (max-width: 768px) {
      video {
        width: 95vw;
        height: 70vh;
      }
    }
  </style>
</head>
<body>

  <div class="container" id="videoFeed">
    <!-- Vídeos serão inseridos via JavaScript -->
  </div>

  <script>
    const showShare = true; // Defina como false para ocultar botão de compartilhar
    const verifiedIcon = "https://cdn-icons-png.flaticon.com/512/12902/12902069.png";

    const videos = [
      {
        src: "video12.mp4",
        username: "usuario1",
        verified: true,
        title: "Primeiro vídeo!",
        audio: "🎵 Música top"
      },
      {
        src: "meuvideo2.mp4",
        username: "usuario2",
        verified: false,
        title: "Segundo vídeo",
        audio: "🎧 Áudio original"
      },
      {
        src: "meuvideo3.mp4",
        username: "usuario3",
        verified: true,
        title: "Terceiro vídeo",
        audio: "🔥 Beat legal"
      },
      {
        src: "meuvideo4.mp4",
        username: "usuario4",
        verified: false,
        title: "Quarto vídeo",
        audio: "🎶 Remix famoso"
      },
      {
        src: "meuvideo5.mp4",
        username: "usuario5",
        verified: true,
        title: "Quinto vídeo",
        audio: "📻 Som ambiente"
      }
    ];

    const container = document.getElementById('videoFeed');

    videos.forEach((vid) => {
      const div = document.createElement('div');
      div.className = 'video-container';

      div.innerHTML = `
        <video src="${vid.src}" autoplay muted loop playsinline></video>

        <div class="info">
          <div class="username">
            @${vid.username}
            ${vid.verified ? `<img src="${verifiedIcon}" alt="Verificado">` : ''}
          </div>
          <div class="title">${vid.title}</div>
          <div class="audio">${vid.audio}</div>
        </div>

        <div class="controls">
          <button onclick="toggleLike(this)">👍</button>
          ${showShare ? `<button onclick="copyLink('${vid.src}')">📤</button>` : ''}
          <button onclick="toggleMute(this)">🔇</button>
        </div>
      `;

      container.appendChild(div);
    });

    // Pausar/despausar ao tocar no vídeo
    document.addEventListener("click", function (e) {
      if (e.target.tagName === "VIDEO") {
        e.target.paused ? e.target.play() : e.target.pause();
      }
    });

    // Mute/Desmute
    function toggleMute(button) {
      const video = button.closest('.video-container').querySelector('video');
      video.muted = !video.muted;
      button.textContent = video.muted ? '🔇' : '🔊';
    }

    // Curtir/descurtir
    function toggleLike(button) {
      const liked = button.classList.toggle("liked");
      button.textContent = liked ? "💕" : "👍";
    }

    // Compartilhar (copiar link)
    function copyLink(src) {
      navigator.clipboard.writeText(src)
        .then(() => alert("📎 Link copiado!"))
        .catch(() => alert("❌ Erro ao copiar link."));
    }
  </script>

</body>
</html>
