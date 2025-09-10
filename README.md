<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Feed estilo TikTok</title>
  <style>
    html, body {
      margin: 0; padding: 0;
      height: 100%;
      overflow: hidden;
      background: black;
      font-family: Arial, sans-serif;
      color: white;
    }
    .container {
      height: 100vh;
      overflow-y: scroll;
      scroll-snap-type: y mandatory;
    }
    .video-wrapper {
      position: relative;
      height: 100vh;
      width: 100vw;
      scroll-snap-align: start;
      display: flex;
      justify-content: center;
      align-items: center;
      background: black;
    }
    video {
      height: 100vh;
      width: 100vw;
      object-fit: cover;
    }
    .buttons {
      position: fixed;
      top: 50%;
      right: 15px;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      gap: 25px;
      z-index: 10;
    }
    .buttons button {
      background: rgba(0,0,0,0.5);
      border: none;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      font-size: 26px;
      color: white;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: background 0.3s;
    }
    .buttons button:hover {
      background: rgba(255,255,255,0.2);
    }
    .like-btn.liked {
      color: #ff2d55;
    }
    /* Botão "Próximo" fixo no canto inferior direito */
    .next-btn {
      position: fixed;
      bottom: 30px;
      right: 20px;
      background: rgba(0,0,0,0.5);
      border: none;
      border-radius: 30px;
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
      color: white;
      z-index: 10;
      user-select: none;
      transition: background 0.3s;
    }
    .next-btn:hover {
      background: rgba(255,255,255,0.2);
    }
  </style>
</head>
<body>

  <div class="container" id="container">
    <div class="video-wrapper">
      <video src="video12.mp4" playsinline></video>
    </div>
    <div class="video-wrapper">
      <video src="video2.mp4" playsinline></video>
    </div>
    <div class="video-wrapper">
      <video src="video3.mp4" playsinline></video>
    </div>
    <div class="video-wrapper">
      <video src="video4.mp4" playsinline></video>
    </div>
    <div class="video-wrapper">
      <video src="video5.mp4" playsinline></video>
    </div>
  </div>

  <div class="buttons">
    <button id="likeBtn" class="like-btn" title="Curtir">👍</button>
    <button id="shareBtn" title="Compartilhar">📤</button>
  </div>

  <button class="next-btn" id="nextBtn">Próximo ▶️</button>

<script>
  const container = document.getElementById('container');
  const videos = Array.from(container.querySelectorAll('video'));
  const likeBtn = document.getElementById('likeBtn');
  const shareBtn = document.getElementById('shareBtn');
  const nextBtn = document.getElementById('nextBtn');

  let currentIndex = 0;
  let likedVideos = new Set();

  function playVideoAt(index) {
    videos.forEach((vid, i) => {
      if(i === index){
        vid.currentTime = 0;
        vid.play();
      } else {
        vid.pause();
      }
    });
    currentIndex = index;
    updateLikeButton();
  }

  function updateLikeButton() {
    if (likedVideos.has(currentIndex)) {
      likeBtn.textContent = "💕";
      likeBtn.classList.add('liked');
    } else {
      likeBtn.textContent = "👍";
      likeBtn.classList.remove('liked');
    }
  }

  // Inicializa o primeiro vídeo
  playVideoAt(0);

  // Controla o scroll para pausar/play vídeo correto
  container.addEventListener('scroll', () => {
    const scrollPos = container.scrollTop;
    const vh = window.innerHeight;
    const newIndex = Math.round(scrollPos / vh);
    if(newIndex !== currentIndex && newIndex >= 0 && newIndex < videos.length) {
      playVideoAt(newIndex);
    }
  });

  // Botão curtir
  likeBtn.addEventListener('click', () => {
    if (likedVideos.has(currentIndex)) {
      likedVideos.delete(currentIndex);
    } else {
      likedVideos.add(currentIndex);
    }
    updateLikeButton();
  });

  // Botão compartilhar (copiar link do vídeo)
  shareBtn.addEventListener('click', () => {
    const currentVideo = videos[currentIndex];
    const src = currentVideo.currentSrc || currentVideo.src;
    navigator.clipboard.writeText(src).then(() => {
      alert("Link copiado para a área de transferência!");
    }).catch(() => {
      alert("Falha ao copiar link.");
    });
  });

  // Botão próximo
  nextBtn.addEventListener('click', () => {
    let nextIndex = currentIndex + 1;
    if(nextIndex >= videos.length) nextIndex = 0;
    container.scrollTo({
      top: nextIndex * window.innerHeight,
      behavior: 'smooth'
    });
    playVideoAt(nextIndex);
  });

</script>
</body>
</html>
