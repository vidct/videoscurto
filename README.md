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
      videoHTML: `<video src="https://v19-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-ve-0068c002/oUWEBSmqIiQ1CK3NDpxiYAbEEQasUEoBPGOLz/?a=1988&bti=ODszNWYuMDE6&ch=0&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C&br=792&bt=396&cs=0&ds=6&ft=4fUEKMXh8Zmo0yeayI4jVohmqpWrKsd.&mime_type=video_mp4&qs=0&rc=NTg6M2U0ZWRoOWY5NGRmNkBpajtyZ205cnV1NTMzNzczM0AyLTAuL2JgXjExNGJiYWIvYSMzMmw0MmQ0L25hLS1kMTZzcw%3D%3D&btag=e00090000&expire=1757689445&l=202509102302414C7A9D0934050D08CACE&ply_type=2&policy=2&signature=3db185ca42d3482cbbc3979fc5d67533&tk=tt_chain_token" muted loop playsinline></video>`
    };

    // Outros vídeos
    const otherVideos = [
      {
        title: "la cocaina",
        username: "PRIVET",
        verified: false,
        showUser: false,
        videoHTML: `<video src="https://v19-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-ve-0068-euttp/oEEF7ktGtABJQAzoE353XVEQwfRlftzC7PhIDJ/?a=1988&bti=ODszNWYuMDE6&ch=0&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C&cv=1&br=598&bt=299&cs=2&ds=3&ft=4fUEKMXh8Zmo0BlxyI4jV4IgDpWrKsd.&mime_type=video_mp4&qs=14&rc=ZjtmPGU2OzpmZWloOTdnZkBpajc2ajg6Zm54bDMzZjczM0A2L2NfMTZeNTQxXmI1NF4yYSMxajNecjRfa29gLS1kMWNzcw%3D%3D&btag=e00090000&expire=1757682141&l=20250910210108847663433A172407AAE8&ply_type=2&policy=2&signature=1f7693ced56a52e4c176a88913291817&tk=tt_chain_token" muted loop playsinline></video>`
      },
      {
        title: "Tutorial de slime",
        username: "slime_master",
        verified: false,
        showUser: false,
        videoHTML: `<video src="https://v19-webapp-prime.tiktok.com/video/tos/alisg/tos-alisg-pve-0037c001/oMeIByLo7QGKNfXAI8cAeoI4QjIijPxDop0cnC/?a=1988&bti=ODszNWYuMDE6&ch=0&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C&cv=1&br=560&bt=280&cs=2&ds=4&ft=4fUEKMXh8Zmo0FiayI4jVV~nXpWrKsd.&mime_type=video_mp4&qs=15&rc=NGg7OzpkOGlnOjk7Ozw7N0BpM3c1OHk5cjRoNDMzODczNEAvYy1jYGJiNV4xNTM2M2FhYSNzLjByMmRrLzVhLS1kMTFzcw%3D%3D&btag=e000b8000&expire=1757689151&l=20250910225848E8CE7C85475EEF10B0D4&ply_type=2&policy=2&signature=f26720a66645d51dde1757a54e23f05e&tk=tt_chain_token" muted loop playsinline></video>`
      },
      {
        title: "Vídeo sem nome visível",
        username: "invisivel123",
        verified: false,
        showUser: false,
        videoHTML: `<video src="https://v16-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-pve-0068/ocBO8FBRJDoWwUEjeQFjASXhQfNDZ3BEMqIDPg/?a=1988&bti=ODszNWYuMDE6&ch=0&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C&cv=1&br=786&bt=393&cs=2&ds=6&ft=4fUEKMXh8Zmo0SOayI4jV5DyXpWrKsd.&mime_type=video_mp4&qs=11&rc=PDg8NTo3NjNnNTQ0Ojk4ZkBpajk8Zng5cmppNjMzNzczM0BfNl80MS4tXy0xYjNhYWE0YSNnLmEzMmRzZS1hLS1kMTZzcw%3D%3D&btag=e00088000&expire=1757688984&l=20250910225531A04EC613CAD7C710A8A7&ply_type=2&policy=2&signature=e96b5dae7b58fc2e574cc5478eaa6bf4&tk=tt_chain_token" muted loop playsinline></video>`
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
