<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vídeo Navegação</title>
  <style>
    body {
      margin: 0;
      background: #000;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }

    .container {
      position: relative;
      width: 360px;
      height: 640px;
      background: #000;
      display: flex;
      flex-direction: column;
      align-items: center;
      overflow: hidden;
    }

    video {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(255,255,255,0.2);
      border: none;
      color: #fff;
      font-size: 18px;
      padding: 10px;
      cursor: pointer;
    }
    .btn:hover { background: rgba(255,255,255,0.4); }

    #prevBtn { left: 5px; display: none; }
    #nextBtn { right: 5px; }

    /* Gostei só aparece em mobile */
    #likeBtn {
      display: none;
      position: absolute;
      bottom: 20px;
      right: 20px;
      font-size: 28px;
      cursor: pointer;
      background: none;
      border: none;
    }

    /* Informações só no mobile */
    .profile-info {
      display: none;
      position: absolute;
      top: 10px;
      left: 10px;
      color: #fff;
      font-size: 14px;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 5px;
    }
    .profile-info img {
      width: 16px;
      height: 16px;
    }

    /* Mobile rules */
    @media (max-width: 768px) {
      #likeBtn { display: block; }
      .profile-info { display: flex; }
    }
  </style>
</head>
<body>

  <div class="container">
    <!-- Vídeo -->
    <video id="video" autoplay muted loop>
      <source src="videos/video1.mp4" type="video/mp4">
    </video>

    <!-- Botões de navegação -->
    <button class="btn" id="prevBtn">Anterior</button>
    <button class="btn" id="nextBtn">Próximo</button>

    <!-- Botão de gostei -->
    <button id="likeBtn">👍</button>

    <!-- Informações do perfil -->
    <div class="profile-info" id="profileInfo">
      <span id="profileName">Nome do Perfil</span>
      <img id="verifiedIcon" src="https://www.kindpng.com/picc/m/256-2569151_instagram-verified-logo-png-transparent-png.png" alt="Verificado">
    </div>
  </div>

  <script>
    // Configurações
    const SHOW_META_ON_MOBILE = true;  // true/false -> mostra título + nome do perfil no mobile
    const SHOW_VERIFIED = true;        // true/false -> mostra o selo verificado

    // Vídeos (altere os paths para suas pastas)
    const videoPaths = [
      "video12.mp4",
      "video2.mp4",
      "videos/video3.mp4"
    ];

    let currentIndex = 0;
    const video = document.getElementById("video");
    const prevBtn = document.getElementById("prevBtn");
    const nextBtn = document.getElementById("nextBtn");
    const likeBtn = document.getElementById("likeBtn");
    const profileInfo = document.getElementById("profileInfo");
    const verifiedIcon = document.getElementById("verifiedIcon");

    // Mostrar/ocultar meta
    profileInfo.style.display = SHOW_META_ON_MOBILE ? "flex" : "none";
    verifiedIcon.style.display = SHOW_VERIFIED ? "inline" : "none";

    // Função para atualizar vídeo
    function updateVideo() {
      video.src = videoPaths[currentIndex];
      prevBtn.style.display = currentIndex === 0 ? "none" : "block";
    }

    // Botões navegação
    nextBtn.addEventListener("click", () => {
      if (currentIndex < videoPaths.length - 1) {
        currentIndex++;
        updateVideo();
      }
    });

    prevBtn.addEventListener("click", () => {
      if (currentIndex > 0) {
        currentIndex--;
        updateVideo();
      }
    });

    // Like toggle
    likeBtn.addEventListener("click", () => {
      likeBtn.textContent = likeBtn.textContent === "👍" ? "💕" : "👍";
    });
  </script>

</body>
</html>
