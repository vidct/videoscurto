<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Mini TikTok Like Only</title>
<style>
  html, body {
    margin: 0; padding: 0;
    height: 100%;
    overflow: hidden;
    background: black;
    font-family: Arial, sans-serif;
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
  }

  video {
    width: 100vw;
    height: 100vh;
    object-fit: cover;
  }

  .like-btn {
    position: fixed;
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(0,0,0,0.5);
    border: none;
    border-radius: 50%;
    width: 56px;
    height: 56px;
    font-size: 28px;
    color: white;
    cursor: pointer;
    z-index: 10;
    transition: background 0.3s;
  }

  .like-btn.liked {
    color: #ff2d55;
    background: rgba(255,45,85,0.8);
  }

</style>
</head>
<body>

<div class="container" id="container">
  <div class="video-wrapper">
    <video src="https://v19-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-ve-0068-euttp/oEEF7ktGtABJQAzoE353XVEQwfRlftzC7PhIDJ/?a=1988&bti=ODszNWYuMDE6&ch=0&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C&cv=1&br=598&bt=299&cs=2&ds=3&ft=4fUEKMXh8Zmo0BlxyI4jV4IgDpWrKsd.&mime_type=video_mp4&qs=14&rc=ZjtmPGU2OzpmZWloOTdnZkBpajc2ajg6Zm54bDMzZjczM0A2L2NfMTZeNTQxXmI1NF4yYSMxajNecjRfa29gLS1kMWNzcw%3D%3D&btag=e00090000&expire=1757682141&l=20250910210108847663433A172407AAE8&ply_type=2&policy=2&signature=1f7693ced56a52e4c176a88913291817&tk=tt_chain_token" playsinline muted loop></video>
  </div>
  <div class="video-wrapper">
    <video src="video2.mp4" playsinline muted loop></video>
  </div>
  <div class="video-wrapper">
    <video src="video3.mp4" playsinline muted loop></video>
  </div>
  <div class="video-wrapper">
    <video src="video4.mp4" playsinline muted loop></video>
  </div>
  <div class="video-wrapper">
    <video src="video5.mp4" playsinline muted loop></video>
  </div>
</div>

<button class="like-btn" id="likeBtn" title="Curtir">👍</button>

<script>
  const container = document.getElementById('container');
  const videos = Array.from(container.querySelectorAll('video'));
  const likeBtn = document.getElementById('likeBtn');

  let currentIndex = 0;
  let likedVideos = new Set();

  function playVideoAt(index) {
    videos.forEach((vid, i) => {
      if(i === index) {
        vid.play();
      } else {
        vid.pause();
        vid.currentTime = 0;
      }
    });
    currentIndex = index;
    updateLikeButton();
  }

  function updateLikeButton() {
    if(likedVideos.has(currentIndex)) {
      likeBtn.textContent = "💕";
      likeBtn.classList.add('liked');
    } else {
      likeBtn.textContent = "👍";
      likeBtn.classList.remove('liked');
    }
  }

  // Inicializa com o primeiro vídeo tocando
  playVideoAt(0);

  // Atualiza vídeo ao scroll
  container.addEventListener('scroll', () => {
    const vh = window.innerHeight;
    const newIndex = Math.round(container.scrollTop / vh);
    if(newIndex !== currentIndex && newIndex >= 0 && newIndex < videos.length) {
      playVideoAt(newIndex);
    }
  });

  // Curtir
  likeBtn.addEventListener('click', () => {
    if(likedVideos.has(currentIndex)) {
      likedVideos.delete(currentIndex);
    } else {
      likedVideos.add(currentIndex);
    }
    updateLikeButton();
  });
</script>

</body>
</html>
