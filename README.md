
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vídeo Navegação com Menu</title>
  <style>
    body {
      margin: 0;
      background: #000;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
      font-family: Arial, sans-serif;
      color: #fff;
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

    /* Popup */
    .popup {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.7);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999;
    }
    .popup-box {
      background: #111;
      padding: 20px;
      border-radius: 12px;
      text-align: center;
      width: 250px;
      box-shadow: 0 0 15px rgba(255,255,255,0.3);
    }
    .popup-box h2 {
      margin: 0 0 15px 0;
    }
    .popup-buttons button {
      margin: 5px;
      padding: 8px 15px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
    }
    .close-btn { background: #555; color: #fff; }
    .goto-btn { background: #00c853; color: #fff; }

    /* Top menu */
    .top-menu {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 30px;
      padding: 10px;
      font-weight: bold;
      font-size: 16px;
      color: #aaa;
      cursor: pointer;
    }
    .top-menu span.active {
      color: #fff;
      border-bottom: 2px solid #fff;
      padding-bottom: 3px;
    }

    /* Player */
    .fy {
      flex: 1;
      width: 100%;
      height: 100%;
      position: relative;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    video {
      width: 100%;
      height: 100%;
      object-fit: cover;
      cursor: pointer;
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

    @media (max-width: 768px) {
      #likeBtn { display: block; }
    }

    /* Explorar */
    .explore {
      flex: 1;
      width: 100%;
      height: 100%;
      display: none;
      flex-wrap: wrap;
      gap: 2px;
      overflow-y: auto;
      padding: 2px;
      box-sizing: border-box;
    }
    .explore-item {
      width: calc(50% - 2px);
      text-align: center;
      color: #fff;
      font-size: 12px;
    }
    .explore video {
      width: 100%;
      height: 180px;
      object-fit: cover;
      display: block;
    }
  </style>
</head>
<body>

  <!-- Popup -->
  <div class="popup" id="welcomePopup">
    <div class="popup-box">
      <h2>Bem-vindo!</h2>
      <div class="popup-buttons">
        <button class="close-btn" id="closePopup">Fechar</button>
        <button class="goto-btn" id="gotoSite">Videos vazados que nao deu pra postar</button>
      </div>
    </div>
  </div>

  <div class="container">
    <!-- Menu -->
    <div class="top-menu">
      <span id="tabFy" class="active">FY</span>
      <span id="tabExplore">Explorar</span>
    </div>

    <!-- FY (player de vídeos aleatórios) -->
    <div class="fy" id="fySection">
      <video id="video" autoplay loop>
        <source src="videos/video1.mp4" type="video/mp4">
      </video>
      <button class="btn" id="prevBtn">Anterior</button>
      <button class="btn" id="nextBtn">Próximo</button>
      <button id="likeBtn">👍</button>
    </div>

    <!-- Explorar (outros vídeos fixos) -->
    <div class="explore" id="exploreSection">
      <div class="explore-item"><video src="videoapanhando.mp4" controls></video>views 5k</div>
      <div class="explore-item"><video src="newsonibus.mp4" controls></video>views 3k</div>
      <div class="explore-item"><video src="videodapraca.mp4" controls></video>views 500 (novo)</div>
      <div class="explore-item"><video src="videos/video1.mp4" controls></video>views 1k</div>
    </div>
  </div>

  <script>
    const allVideos = [
      "video12.mp4",
      "video1.mp4",
      "video7.mp4",
      "video13.mp4",
      "video14.mp4",
      "video5.mp4",
      "video8.mp4",
      "video9.mp4",
      "video10.mp4",
      "video11.mp4",
      "video6.mp4",
      "video2.mp4",
      "video4.mp4",
      "video3.mp4"
    ];

    let playlist = [];
    let history = [];
    let currentIndex = -1;

    const video = document.getElementById("video");
    const prevBtn = document.getElementById("prevBtn");
    const nextBtn = document.getElementById("nextBtn");
    const likeBtn = document.getElementById("likeBtn");

    // Embaralhar array
    function shuffle(array) {
      let m = array.length, t, i;
      while (m) {
        i = Math.floor(Math.random() * m--);
        t = array[m];
        array[m] = array[i];
        array[i] = t;
      }
      return array;
    }

    function refillPlaylist() {
      playlist = shuffle([...allVideos]);
    }

    function updateVideo(src) {
      video.src = src;
      video.play();
      prevBtn.style.display = history.length > 1 ? "block" : "none";
    }

    nextBtn.addEventListener("click", () => {
      if (playlist.length === 0) refillPlaylist();
      const nextVideo = playlist.shift();
      history.push(nextVideo);
      currentIndex = history.length - 1;
      updateVideo(nextVideo);
    });

    prevBtn.addEventListener("click", () => {
      if (currentIndex > 0) {
        currentIndex--;
        const prevVideo = history[currentIndex];
        updateVideo(prevVideo);
      }
    });

    likeBtn.addEventListener("click", () => {
      likeBtn.textContent = likeBtn.textContent === "👍" ? "💕" : "👍";
    });

    video.addEventListener("click", () => {
      if (video.paused) video.play();
      else video.pause();
    });

    // Inicializa
    refillPlaylist();
    nextBtn.click();

    // Tabs
    const tabFy = document.getElementById("tabFy");
    const tabExplore = document.getElementById("tabExplore");
    const fySection = document.getElementById("fySection");
    const exploreSection = document.getElementById("exploreSection");

    tabFy.addEventListener("click", () => {
      tabFy.classList.add("active");
      tabExplore.classList.remove("active");
      fySection.style.display = "flex";
      exploreSection.style.display = "none";
    });

    tabExplore.addEventListener("click", () => {
      tabExplore.classList.add("active");
      tabFy.classList.remove("active");
      exploreSection.style.display = "flex";
      fySection.style.display = "none";
    });

    // Popup
    const popup = document.getElementById("welcomePopup");
    const closePopup = document.getElementById("closePopup");
    const gotoSite = document.getElementById("gotoSite");

    closePopup.addEventListener("click", () => {
      popup.style.display = "none";
    });

    gotoSite.addEventListener("click", () => {
      window.location.href = "https://4br.me/Videoscurto"; // troque para sua URL
    });
  </script>

</body>
</html>
