<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Feed estilo TikTok ajustado</title>
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
    scroll-behavior: smooth;
  }

  .video-wrapper {
    height: 100vh;
    width: 100vw;
    scroll-snap-align: start;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    background: black;
  }

  video {
    width: 100vw;
    height: 100vh;
    object-fit: cover;
  }

  /* Botões fixos nas laterais, alinhados verticalmente no centro */
  .buttons {
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    z-index: 20;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .buttons.left {
    left: 10px;
  }

  .buttons.right {
    right: 10px;
  }

  .buttons button {
    background: rgba(0,0,0,0.4);
    border: none;
    border-radius: 50%;
    width: 48px;
    height: 48px;
    font-size: 24px;
    color: white;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: background 0.3s ease;
  }

  .buttons button:hover {
    background: rgba(255,255,255,0.2);
  }

  .like-btn.liked {
    color: #ff2d55;
  }

  /* Botão próximo fixo no canto inferior direito */
  .next-btn {
    position: fixed;
    bottom: 30px;
    right: 20px;
    background: rgba(0,0,0,0.5);
    border: none;
    border-radius: 30px;
    padding: 10px 18px;
    font-size: 18px;
    cursor: pointer;
    color: white;
    z-index: 20;
    user-select: none;
    transition: background 0.3s ease;
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

<!-- Botões nas laterais -->
<div class="buttons left">
  <button id="likeBtn" class="like-btn" title="Curtir">👍</button>
</div>

<div class="buttons right">
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
      if (i === index) {
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

  // Inicia o primeiro vídeo
  playVideoAt(0);

  // Detecta o vídeo atual baseado no scroll
  container.addEventListener('scroll', () => {
    const scrollTop = container.scrollTop;
    const vh = window.innerHeight;
    const newIndex = Math.round(scrollTop / vh);
    if (newIndex !== currentIndex && newIndex >= 0 && newIndex < videos.length) {
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

  // Botão compartilhar: copia link do vídeo
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
    if (nextIndex >= videos.length) nextIndex = 0;
    container.scrollTo({
      top: nextIndex * window.innerHeight,
      behavior: 'smooth'
    });
    playVideoAt(nextIndex);
  });
</script>

</body>
</html>
