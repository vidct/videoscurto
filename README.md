<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Feed de Vídeos Personalizável</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background-color: #000;
      font-family: sans-serif;
      overflow-x: hidden;
    }

    .container {
      scroll-snap-type: y mandatory;
      overflow-y: scroll;
      height: 100vh;
    }

    .video {
      width: 100vw;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      scroll-snap-align: start;
      position: relative;
      color: white;
    }

    video, iframe {
      max-width: 100%;
      max-height: 80%;
    }

    .info {
      position: absolute;
      bottom: 80px;
      left: 20px;
      text-align: left;
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

    .title {
      font-size: 18px;
      margin-top: 4px;
    }

    .like-btn, .share-btn {
      position: absolute;
      bottom: 20px;
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
      color: white;
    }

    .like-btn {
      right: 20px;
    }

    .share-btn {
      right: 70px;
    }

    .like-btn.liked {
      color: pink;
    }
  </style>
</head>
<body>
  <div class="container" id="videoContainer"></div>

  <script>
    const verifiedIcon = 'https://cdn-icons-png.flaticon.com/512/12902/12902069.png';

    // Vídeo fixo no topo
    const fixedVideo = {
      title: "🎯 Vídeo Especial de Destaque",
      username: "VIDCT",
      verified: true,
      showUser: true,
      videoHTML: `<video src="video12.mp4" muted loop playsinline></video>`
    };

    // Outros vídeos
    const otherVideos = [
      {
        title: "la cocaina",
        username: "PRIVET",
        verified: false,
        showUser: false,
        videoHTML: `<video src="https://v19-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-ve-0068-euttp/oEEF7ktGtABJQAzoE353XVEQ..." muted loop playsinline></video>`
      },
      {
        title: "Tutorial de slime",
        username: "slime_master",
        verified: false,
        showUser: false,
        videoHTML: `<video src="https://v19-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-pve-0068/o8PAU0auHGQxEUSKmCvNDvzB..." muted loop playsinline></video>`
      },
      {
        title: "Vídeo sem nome visível",
        username: "invisivel123",
        verified: false,
        showUser: false,
        videoHTML: `<video src="meuvideo4.mp4" muted loop playsinline></video>`
      },
      // Adicione até 25 vídeos como quiser aqui
    ];

    // Embaralhar os outros vídeos
    function shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }

    shuffle(otherVideos);
    const allVideos = [fixedVideo, ...otherVideos];

    const container = document.getElementById('videoContainer');

    allVideos.forEach((item) => {
      const videoDiv = document.createElement('div');
      videoDiv.className = 'video';

      videoDiv.innerHTML = `
        ${item.videoHTML}
        <div class="info">
          ${item.showUser ? `
            <div class="username">
              @${item.username}
              ${item.verified ? `<img src="${verifiedIcon}" alt="Verificado">` : ''}
            </div>` : ''}
          <div class="title">${item.title}</div>
        </div>
        <button class="like-btn" onclick="toggleLike(this)">👍</button>
        <button class="share-btn" onclick="shareVideo(this)">📤</button>
      `;

      container.appendChild(videoDiv);
    });

    function toggleLike(button) {
      const liked = button.classList.toggle('liked');
      button.innerText = liked ? '💕' : '👍';
    }

    function shareVideo(button) {
      const videoElement = button.parentElement.querySelector('video');
      if (videoElement && videoElement.src) {
        navigator.clipboard.writeText(videoElement.src)
          .then(() => {
            alert("📎 Link do vídeo copiado!");
          })
          .catch(() => {
            alert("❌ Não foi possível copiar o link.");
          });
      } else {
        alert("⚠️ Vídeo não encontrado.");
      }
    }

    // ▶️⏸️ Pause automático com visibilidade
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        const video = entry.target;
        if (entry.isIntersecting) {
          video.play().catch(() => {});
        } else {
          video.pause();
        }
      });
    }, {
      threshold: 0.8
    });

    document.querySelectorAll('video').forEach(video => {
      observer.observe(video);
    });
  </script>
</body>
</html>
