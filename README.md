<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Vídeo TikTok</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background-color: #000;
      font-family: sans-serif;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      height: 100vh;
    }

    video {
      max-height: 70vh;
      max-width: 100%;
      border-radius: 10px;
    }

    .info {
      margin-top: 15px;
      text-align: center;
    }

    .username {
      font-weight: bold;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 5px;
    }

    .username img {
      width: 16px;
      height: 16px;
    }

    .title {
      margin-top: 5px;
      font-size: 18px;
    }

    .buttons {
      margin-top: 20px;
      display: flex;
      gap: 20px;
    }

    button {
      font-size: 24px;
      background: none;
      border: none;
      color: white;
      cursor: pointer;
    }

    .liked {
      color: pink;
    }
  </style>
</head>
<body>

  <video
    src="https://v19-webapp-prime.tiktok.com/video/tos/useast2a/tos-useast2a-ve-0068-euttp/oEEF7ktGtABJQAzoE353XVEQwfRlftzC7PhIDJ/?a=1988&bti=ODszNWYuMDE6&ch=0&cr=3&dr=0&lr=all&cd=0%7C0%7C0%7C&cv=1&br=598&bt=299&cs=2&ds=3&ft=4fUEKMXh8Zmo0BlxyI4jV4IgDpWrKsd.&mime_type=video_mp4&qs=14&rc=ZjtmPGU2OzpmZWloOTdnZkBpajc2ajg6Zm54bDMzZjczM0A2L2NfMTZeNTQxXmI1NF4yYSMxajNecjRfa29gLS1kMWNzcw%3D%3D&btag=e00090000&expire=1757682141&l=20250910210108847663433A172407AAE8&ply_type=2&policy=2&signature=1f7693ced56a52e4c176a88913291817&tk=tt_chain_token"
    autoplay
    muted
    loop
    playsinline
  ></video>

  <div class="info">
    <div class="username">
      @PRIVET
      <img src="https://cdn-icons-png.flaticon.com/512/12902/12902069.png" alt="Verificado">
    </div>
    <div class="title">la cocaina</div>
  </div>

  <div class="buttons">
    <button class="like-btn" onclick="toggleLike(this)">👍</button>
    <button onclick="copyLink()">📤</button>
  </div>

  <script>
    function toggleLike(btn) {
      const liked = btn.classList.toggle('liked');
      btn.textContent = liked ? '💕' : '👍';
    }

    function copyLink() {
      const video = document.querySelector('video');
      const link = video?.src;
      if (link) {
        navigator.clipboard.writeText(link)
          .then(() => alert("📎 Link copiado!"))
          .catch(() => alert("❌ Erro ao copiar o link."));
      }
    }
  </script>

</body>
</html>
