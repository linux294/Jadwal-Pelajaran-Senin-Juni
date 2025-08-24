<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pesan Khusus</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      text-align: center;
      overflow: hidden;
    }

    /* Welcome */
    .welcome { animation: fadeIn 2s ease-in; }
    .welcome h1 { font-size: 3rem; margin-bottom: 30px; }
    .welcome p { font-size: 1.5rem; margin-bottom: 20px; }
    .loading { font-size: 1.2rem; animation: pulse 2s infinite; }

    /* Jumpscare */
    .jumpscare {
      display: none;
      position: fixed; top: 0; left: 0;
      width: 100vw; height: 100vh;
      background: black;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      z-index: 9999;
    }
    .jumpscare.active { display: flex; animation: shake 0.5s infinite; }
    .jumpscare h1 {
      color: red; font-size: 4rem; text-shadow: 0 0 25px red;
      margin-bottom: 20px; animation: textShake 0.1s infinite;
    }
    .jumpscare img {
      width: 400px; height: auto; border-radius: 10px;
      box-shadow: 0 0 50px red; margin: 20px 0;
      animation: textShake 0.1s infinite;
    }
    .jumpscare p {
      color: white; font-size: 2rem;
      background: rgba(255,0,0,0.8);
      padding: 15px 25px; border-radius: 10px; margin-top: 20px;
      animation: textShake 0.1s infinite;
    }
    .close-btn {
      margin-top: 40px; padding: 15px 30px;
      background: rgba(255,255,255,0.2); color: white;
      border: 2px solid white; border-radius: 25px;
      cursor: pointer; font-size: 1.2rem; font-weight: bold;
    }
    .close-btn:hover { background: rgba(255,255,255,0.4); }

    /* Animations */
    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    @keyframes pulse { 0%,100% { opacity: 0.5; } 50% { opacity: 1; } }
    @keyframes shake {
      0% { transform: translate(1px, 1px); }
      10% { transform: translate(-1px, -2px); }
      20% { transform: translate(-3px, 0px); }
      30% { transform: translate(3px, 2px); }
      40% { transform: translate(1px, -1px); }
      50% { transform: translate(-1px, 2px); }
      60% { transform: translate(-3px, 1px); }
      70% { transform: translate(3px, 1px); }
      80% { transform: translate(-1px, -1px); }
      90% { transform: translate(1px, 2px); }
      100% { transform: translate(1px, -2px); }
    }
    @keyframes textShake {
      0% { transform: translate(0); }
      25% { transform: translate(5px, 5px); }
      50% { transform: translate(-5px, -5px); }
      75% { transform: translate(5px, -5px); }
      100% { transform: translate(-5px, 5px); }
    }
  </style>
</head>
<body>
  <!-- Welcome Screen -->
  <div class="welcome" id="welcome">
    <h1>Pesan Khusus</h1>
    <p>Dari: Faishal Amir Khallis</p>
    <p>Untuk: Kalian Semua</p>
    <div class="loading">Klik layar untuk memuat pesan...</div>
  </div>

  <!-- Jumpscare Screen -->
  <div class="jumpscare" id="jumpscare">
    <h1>DARI FAISHAL AMIR KHALLIS!</h1>
    <!-- Ganti gambar hantu sesuai file Anda -->
    <img src="hantu.jpg" alt="Hantu Seram">
    <p>UNTUK KALIAN SEMUA!</p>
    <button class="close-btn" onclick="closeScare()">Tutup</button>
  </div>

  <!-- Audio -->
  <audio id="audio" preload="auto">
    <source src="scream.mp3" type="audio/mpeg">
  </audio>

  <script>
    const audio = document.getElementById("audio");

    function setupAudio() {
      audio.volume = 1.0;
      audio.muted = false;
      // Mulai & pause sekali untuk unlock audio
      audio.play().then(() => {
        audio.pause();
        audio.currentTime = 0;
        console.log("Audio siap dimainkan!");
      }).catch(() => {
        console.log("Browser blokir autoplay, tapi sudah unlock setelah klik.");
      });
    }

    async function playAudio() {
      try {
        audio.currentTime = 0;
        await audio.play();
      } catch (e) {
        console.log("Audio gagal:", e);
      }
    }

    function jumpscare() {
      const welcome = document.getElementById("welcome");
      const scare = document.getElementById("jumpscare");

      welcome.style.display = "none";
      scare.classList.add("active");

      playAudio();
      setTimeout(playAudio, 200);
      setTimeout(playAudio, 400);

      // Flash background
      document.body.style.background = "red";
      setTimeout(() => document.body.style.background = "black", 100);
      setTimeout(() => document.body.style.background = "red", 200);
      setTimeout(() => document.body.style.background = "black", 300);
    }

    function closeScare() {
      const welcome = document.getElementById("welcome");
      const scare = document.getElementById("jumpscare");

      audio.pause();
      audio.currentTime = 0;

      scare.classList.remove("active");
      welcome.style.display = "block";
      document.body.style.background =
        "linear-gradient(135deg, #667eea 0%, #764ba2 100%)";
    }

    document.addEventListener("DOMContentLoaded", function () {
      // User harus klik dulu → unlock audio → jalankan jumpscare
      document.addEventListener("click", function () {
        setupAudio();
        setTimeout(jumpscare, 3000); // 3 detik setelah klik
      }, { once: true });
    });
  </script>
</body>
</html>
