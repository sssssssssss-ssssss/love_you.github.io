# love_you.github.io
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Я люблю тебя</title>
  <style>
    :root {
      --pink-1: #FFB6C1;
      --pink-2: #FF69B4;
      --white: #ffffff;
    }

    * { 
      margin: 0; 
      padding: 0; 
      box-sizing: border-box; 
    }

    body {
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      /* Красивый розовый градиент */
      background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
      font-family: 'Arial', sans-serif;
      overflow: hidden;
      position: relative;
    }

    /* Панель управления музыкой (справа сверху) */
    .music-panel {
      position: fixed;
      top: 20px;
      right: 20px;
      background: rgba(255, 255, 255, 0.95);
      padding: 15px;
      border-radius: 12px;
      box-shadow: 0 8px 32px rgba(0,0,0,0.2);
      display: flex;
      align-items: center;
      gap: 12px;
      z-index: 100;
      backdrop-filter: blur(5px);
      border: 1px solid rgba(255,255,255,0.5);
    }

    .control-btn {
      width: 45px;
      height: 45px;
      border-radius: 50%;
      border: none;
      background-color: var(--pink-2);
      color: white;
      font-size: 20px;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: transform 0.2s, box-shadow 0.2s;
    }

    .control-btn:hover { 
      transform: scale(1.1); 
      box-shadow: 0 0 15px var(--pink-2); 
    }
    
    .control-btn.paused { 
      background-color: #ccc; 
      color: #555; 
    }
    
    .volume-container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .volume-label {
      font-size: 10px;
      text-transform: uppercase;
      color: #666;
      margin-bottom: 4px;
      font-weight: bold;
    }

    input[type=range] {
      cursor: pointer;
      accent-color: var(--pink-2); /* Цвет ползунка под тему */
      width: 80px;
    }

    .container {
      text-align: center;
      position: relative;
      z-index: 10;
    }

    h1 {
      color: var(--white);
      font-size: 4rem;
      text-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
      line-height: 1.1;
      margin-bottom: 20px;
      opacity: 0;
      transform: translateY(30px);
      animation: fadeInUp 1s ease forwards 0.5s;
      text-transform: uppercase;
      letter-spacing: 3px;
    }

    p.subtitle {
      color: rgba(255, 255, 255, 0.8);
      font-size: 1.2rem;
      margin-top: 10px;
      opacity: 0;
      animation: fadeIn 1s ease forwards 1s;
    }

    @keyframes fadeInUp { 
      to { 
        opacity: 1; 
        transform: translateY(0); 
      } 
    }

    @keyframes fadeIn { 
      to { 
        opacity: 1; 
      } 
    }

    /* Падающие сердечки (CSS-анимация, без картинок) */
    .heart {
      position: absolute;
      top: -50px;
      width: 30px;
      height: 30px;
      background-color: var(--pink-2);
      /* Форма сердца через clip-path */
      clip-path: path('M10,30 A20,20 0,0,1 30,30 A20,20 0,0,1 20,10 Z');
      filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
      animation: float 6s infinite ease-in-out;
      will-change: transform;
    }
    /* Задержка анимации для каждого сердечка, чтобы они падали не синхронно */
    .heart:nth-child(2) { animation-delay: 1s; }
    .heart:nth-child(3) { animation-delay: 2s; }
    .heart:nth-child(4) { animation-delay: 3s; }
    .heart:nth-child(5) { animation-delay: 4s; }

    @keyframes float {
      0%, 100% { transform: translate(0, -50px) rotate(0deg); }
      50% { transform: translate(15px, -80px) rotate(-10deg); }
    }

    /* Адаптив для мобильных телефонов */
    @media (max-width: 768px) {
      h1 { font-size: 2.5rem; }
      .heart { width: 20px; height: 20px; }
      .music-panel { 
        top: 15px; 
        right: 15px; 
        padding: 10px; 
        gap: 8px; 
      }
      .control-btn { 
        width: 40px; 
        height: 40px; 
        font-size: 18px; 
      }
    }
  </style>
</head>
<body>

  <!-- Панель управления -->
  <div class="music-panel">
    <button id="playBtn" class="control-btn" title="Включить/Выключить музыку">▶</button>
    <div class="volume-container">
      <span class="volume-label">Громкость</span>
      <!-- Ползунок от 0 до 1, шаг 0.1, по умолчанию 0.3 (тихо) -->
      <input type="range" id="volumeSlider" min="0" max="1" step="0.1" value="0.3">
    </div>
  </div>

  <!-- Аудиофайл -->
  <!-- ВАЖНО: src должен точно совпадать с именем файла: ksb_song.mp3 -->
  <audio id="bgMusic" loop>
    <source src="ksb_song.mp3" type="audio/mpeg">
    Ваш браузер не поддерживает аудио.
  </audio>

  <!-- Декоративные сердечки -->
  <div class="heart"></div>
  <div class="heart"></div>
  <div class="heart"></div>
  <div class="heart"></div>
  <div class="heart"></div>

  <!-- Основной текст -->
  <div class="container">
    <h1>Я люблю тебя</h1>
    <p class="subtitle">Ты — самое дорогое, что у меня есть</p>
  </div>

  <script>
    const playBtn = document.getElementById('playBtn');
    const bgMusic = document.getElementById('bgMusic');
    const volumeSlider = document.getElementById('volumeSlider');
    let isPlaying = false;

    // Логика кнопки Play/Pause
    playBtn.addEventListener('click', () => {
      if (!isPlaying) {
        // Пытаемся запустить музыку
        const playPromise = bgMusic.play();
        
        if (playPromise !== undefined) {
          playPromise
            .then(_ => {
              isPlaying = true;
              playBtn.textContent = '⏸'; // Меняем иконку на "Пауза"
              playBtn.classList.remove('paused');
            })
            .catch(error => {
              console.log('Автовоспроизведение заблокировано браузером. Нужно кликнуть по странице.', error);
              // В некоторых браузерах нужно сначала кликнуть по любому месту страницы
            });
        }
      } else {
        bgMusic.pause();
        isPlaying = false;
        playBtn.textContent = '▶'; // Возвращаем иконку "Play"
        playBtn.classList.add('paused');
      }
    });

    // Логика изменения громкости
    volumeSlider.addEventListener('input', (e) => {
      bgMusic.volume = e.target.value;
    });
  </script>
</body>
</html>
