<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Para o amor da minha vida</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Great+Vibes&family=Montserrat:wght@400;700&display=swap');

    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      overflow: hidden;
      background: #b22222; /* fundo vermelho escuro */
      font-family: 'Montserrat', sans-serif;
      color: white;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      pointer-events: none;
      z-index: -1;
    }

    .container {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      max-width: 600px;
      width: 90%;
      background: rgba(255 255 255 / 0.15);
      border-radius: 20px;
      padding: 20px 30px 40px 30px;
      backdrop-filter: blur(8px);
      text-align: center;
      box-shadow: 0 0 30px rgba(255 192, 203, 0.6);
      animation: fadeIn 2s ease forwards;
      opacity: 0;
    }

    @keyframes fadeIn {
      to {
        opacity: 1;
      }
    }

    .polaroid {
      background: white;
      padding: 20px 20px 60px 20px;
      border-radius: 15px;
      box-shadow: 0 0 40px rgba(255, 105, 180, 0.7);
      cursor: pointer;
      transition: transform 0.3s ease;
      animation: fadeIn 2s ease forwards;
      opacity: 0;
      animation-delay: 1s;
      animation-fill-mode: forwards;
    }

    .polaroid:hover {
      transform: scale(1.05);
      box-shadow: 0 0 60px rgba(255, 105, 180, 1);
    }

    .polaroid img {
      width: 100%;
      border-radius: 12px;
      display: block;
      margin: 0 auto;
      animation: pulse 3s infinite;
      /* pulso suave para dar vida */
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.03); }
    }

    .caption {
      margin-top: 15px;
      font-size: 18px;
      color: #a94442;
      font-weight: 700;
      font-family: 'Great Vibes', cursive;
    }

    .text-below {
      margin-top: 40px;
      font-size: 22px;
      line-height: 1.6;
      color: #ffe4e1;
      font-family: 'Great Vibes', cursive;
      font-weight: 500;
      letter-spacing: 0.03em;
      animation: fadeIn 2.5s ease forwards;
      opacity: 0;
      animation-delay: 1.5s;
      animation-fill-mode: forwards;
      text-shadow: 0 0 8px #ff6fa3;
    }

    /* pequenas estrelas piscando no fundo */
    .star {
      position: fixed;
      background: white;
      border-radius: 50%;
      opacity: 0.8;
      animation: twinkle 4s infinite alternate;
      pointer-events: none;
      z-index: -1;
    }

    @keyframes twinkle {
      0% {opacity: 0.3;}
      100% {opacity: 1;}
    }
  </style>
</head>
<body>

<canvas id="heartCanvas"></canvas>

<div class="container">
  <div class="polaroid" onclick="togglePhoto()">
    <img id="photo" src="https://i.postimg.cc/HVxrX9W2/pretoebranco.jpg" alt="Foto do casal" />
    <div class="caption">clique na foto e veja a mágica ✨</div>
  </div>
  <div class="text-below">
    você na minha vida é tão especial assim como o impala é especial para o Dean, você chegou na minha vida e a mudou por completo, meus dias passaram a ter mais cor e a minha vida passou a ser mais feliz, não pude comprar um presente mas fiz isso para você. saiba que eu te amo muito meu amor da minha vida 🩷✨️
  </div>
</div>

<!-- Áudio romântico em loop (volume baixo) -->
<audio autoplay loop muted id="romantic-audio" src="https://cdn.pixabay.com/download/audio/2022/03/26/audio_c87b0609d4.mp3?filename=soft-piano-melody-12614.mp3"></audio>

<script>
  const canvas = document.getElementById('heartCanvas');
  const ctx = canvas.getContext('2d');

  let hearts = [];
  let miniExplosions = [];

  function resize() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
  window.addEventListener('resize', resize);
  resize();

  function createHeart(x = Math.random() * canvas.width, y = -20, size = 10 + Math.random() * 20, speed = 1 + Math.random() * 3) {
    return { x, y, size, speed, alpha: 0.5 + Math.random() * 0.5 };
  }

  function drawHeart(x, y, size, alpha) {
    ctx.save();
    ctx.translate(x, y);
    ctx.scale(size / 30, size / 30);
    ctx.shadowColor = 'rgba(255, 105, 180, 0.8)';
    ctx.shadowBlur = 15;
    ctx.beginPath();
    ctx.moveTo(0, 0);
    ctx.bezierCurveTo(0, -3, -5, -15, -15, -15);
    ctx.bezierCurveTo(-35, -15, -35, 10, -35, 10);
    ctx.bezierCurveTo(-35, 25, -20, 40, 0, 50);
    ctx.bezierCurveTo(20, 40, 35, 25, 35, 10);
    ctx.bezierCurveTo(35, 10, 35, -15, 15, -15);
    ctx.bezierCurveTo(5, -15, 0, -3, 0, 0);
    ctx.closePath();
    ctx.fillStyle = `rgba(255, 105, 180, ${alpha})`;
    ctx.fill();
    ctx.restore();
  }

  function createMiniHearts(x, y) {
    const miniHearts = [];
    for (let i = 0; i < 15; i++) {
      miniHearts.push({
        x,
        y,
        size: 5 + Math.random() * 5,
        speedX: (Math.random() - 0.5) * 4,
        speedY: (Math.random() - 0.5) * 4,
        alpha: 1,
        life: 60
      });
    }
    return miniHearts;
  }

  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // cria corações caindo com brilho
    if (Math.random() < 0.3) {
      hearts.push(createHeart());
    }

    hearts.forEach((heart, i) => {
      heart.y += heart.speed;
      drawHeart(heart.x, heart.y, heart.size, heart.alpha);
      if (heart.y > canvas.height) hearts.splice(i, 1);
    });

    miniExplosions.forEach((group, groupIndex) => {
      group.forEach((mini, i) => {
        mini.x += mini.speedX;
        mini.y += mini.speedY;
        mini.alpha -= 0.02;
        mini.life--;
        drawHeart(mini.x, mini.y, mini.size, mini.alpha);
        if (mini.life <= 0) group.splice(i, 1);
      });
      if (group.length === 0) miniExplosions.splice(groupIndex, 1);
    });

    requestAnimationFrame(animate);
  }

  function explodeBigHearts() {
    for (let i = 0; i < 4; i++) {
      const x = Math.random() * canvas.width * 0.9 + 20;
      const y = Math.random() * canvas.height * 0.5 + 50;
      drawHeart(x, y, 35, 1);
      miniExplosions.push(createMiniHearts(x, y));
    }
  }

  setInterval(explodeBigHearts, 5000);
  animate();

  // Troca de foto
  let isColor = false;
  function togglePhoto() {
    const photo = document.getElementById('photo');
    if (!isColor) {
      photo.src = 'https://i.postimg.cc/s11zJ6Sv/colorida.jpg';
    } else {
      photo.src = 'https://i.postimg.cc/HVxrX9W2/pretoebranco.jpg';
    }
    isColor = !isColor;
  }

  // Cria algumas estrelas piscando no fundo para efeito romântico
  function createStars(numStars = 50) {
    for (let i = 0; i < numStars; i++) {
      const star = document.createElement('div');
      star.classList.add('star');
      star.style.width = `${Math.random() * 3 + 1}px`;
      star.style.height = star.style.width;
      star.style.top = `${Math.random() * window.innerHeight}px`;
      star.style.left = `${Math.random() * window.innerWidth}px`;
      star.style.animationDuration = `${2 + Math.random() * 3}s`;
      star.style.animationDelay = `${Math.random() * 5}s`;
      document.body.appendChild
