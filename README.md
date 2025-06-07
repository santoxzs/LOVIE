<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Para Meu Amor ❤️</title>
  <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
  <style>
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      background: #b22222;
      overflow: hidden;
      font-family: 'Montserrat', sans-serif;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
    }

    .container {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(10px);
      padding: 30px;
      border-radius: 20px;
      text-align: center;
      animation: fadeIn 2s ease forwards;
      opacity: 0;
      max-width: 90%;
      width: 400px;
      color: white;
    }

    .polaroid {
      background: white;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      margin-bottom: 20px;
    }

    .polaroid img {
      width: 100%;
      max-width: 350px;
      border-radius: 5px;
      cursor: pointer;
      animation: pulse 3s infinite;
      display: block;
      margin: 0 auto;
    }

    .caption {
      font-family: 'Great Vibes', cursive;
      font-size: 1.2em;
      margin-top: 10px;
      color: #b22222;
    }

    .text-below {
      font-family: 'Great Vibes', cursive;
      color: white;
      font-size: 1.5em;
      text-shadow: 1px 1px 3px rgba(0,0,0,0.6);
      margin-bottom: 25px;
      line-height: 1.3;
    }

    .spotify-player {
      margin: 0 auto 15px auto;
      border-radius: 12px;
      overflow: hidden;
      width: 100%;
      max-width: 350px;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }

    .lyrics {
      font-style: italic;
      font-size: 1em;
      color: #ffe4e1;
      text-align: center;
      line-height: 1.3;
      user-select: none;
    }

    @keyframes fadeIn {
      to { opacity: 1; }
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.03); }
    }
  </style>
</head>
<body>
  <canvas id="heartCanvas"></canvas>

  <div class="container">
    <div class="polaroid">
      <img id="mainImage" src="https://i.postimg.cc/HVxrX9W2/foto-pb.jpg" alt="Casal"/>
      <div class="caption">Clique na foto e veja a mágica ✨</div>
    </div>
    <div class="text-below">
      você na minha vida é tão especial assim como o impala é especial para o Dean, você chegou na minha vida e a mudou por completo, meus dias passaram a ter mais cor e a minha vida passou a ser mais feliz, não pude comprar um presente mas fiz isso para você. saiba que eu te amo muito meu amor da minha vida 🩷✨️
    </div>

    <div class="spotify-player">
      <iframe 
        src="https://open.spotify.com/embed/track/2QjOHCTQ1Jl3zawyYOpxh6?utm_source=generator" 
        width="100%" 
        height="80" 
        frameborder="0" 
        allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" 
        loading="lazy"
        allowfullscreen>
      </iframe>
    </div>

    <div class="lyrics">
      “Oh, she knows what I think about<br/>
      And what I think about<br/>
      One love, two mouths<br/>
      One love, one house<br/>
      No shirt, no blouse<br/>
      Just us, you find out<br/>
      Nothing that I wouldn't wanna tell you about, no<br/>
      'Cause it's too cold<br/>
      For you here<br/>
      And now, so let me hold<br/>
      Both your hands in the holes of my sweater”
    </div>
  </div>

  <script>
    const canvas = document.getElementById('heartCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    window.addEventListener('resize', () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });

    let hearts = [], bigHearts = [], stars = [];

    function createHeart(x, y, size, dx, dy, color) {
      return { x, y, size, dx, dy, color };
    }

    function drawHeart(x, y, size, color) {
      ctx.save();
      ctx.translate(x, y);
      ctx.scale(size, size);
      ctx.beginPath();
      ctx.moveTo(0, 0);
      ctx.bezierCurveTo(0, -3, -3, -3, -3, 0);
      ctx.bezierCurveTo(-3, 3, 0, 5, 0, 6);
      ctx.bezierCurveTo(0, 5, 3, 3, 3, 0);
      ctx.bezierCurveTo(3, -3, 0, -3, 0, 0);
      ctx.fillStyle = color;
      ctx.shadowColor = color;
      ctx.shadowBlur = 10;
      ctx.fill();
      ctx.restore();
    }

    function createStars(count) {
      for (let i = 0; i < count; i++) {
        stars.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          radius: Math.random() * 1.5 + 0.5,
          alpha: Math.random(),
          dAlpha: (Math.random() * 0.02 + 0.005) * (Math.random() < 0.5 ? -1 : 1)
        });
      }
    }

    function drawStars() {
      stars.forEach(s => {
        s.alpha += s.dAlpha;
        if (s.alpha <= 0 || s.alpha >= 1) s.dAlpha *= -1;
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.radius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255, 255, 255, ${s.alpha})`;
        ctx.fill();
      });
    }

    function animate() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawStars();

      // Mini corações (chuva)
      for (let i = 0; i < 2; i++) {
        hearts.push(createHeart(
          Math.random() * canvas.width,
          -10,
          Math.random() * 0.5 + 0.5,
          0,
          Math.random() * 1 + 1,
          'pink'
        ));
      }

      hearts.forEach((h, index) => {
        h.y += h.dy;
        drawHeart(h.x, h.y, h.size, h.color);
        if (h.y > canvas.height) hearts.splice(index, 1);
      });

      // Explosões de corações
      bigHearts.forEach((b, index) => {
        b.x += b.dx;
        b.y += b.dy;
        drawHeart(b.x, b.y, b.size, b.color);
        b.life--;
        if (b.life <= 0) bigHearts.splice(index, 1);
      });

      requestAnimationFrame(animate);
    }

    function explodeBigHearts() {
      for (let i = 0; i < 4; i++) {
        const bx = Math.random() * canvas.width;
        const by = Math.random() * canvas.height / 2;
        for (let j = 0; j < 15; j++) {
          const angle = Math.random() * 2 * Math.PI;
          const speed = Math.random() * 2 + 1;
          bigHearts.push({
            x: bx,
            y: by,
            dx: Math.cos(angle) * speed,
            dy: Math.sin(angle) * speed,
            size: Math.random() * 0.3 + 0.3,
            color: 'hotpink',
            life: 60
          });
        }
      }
    }

    // Trocar imagem P&B por colorida ao clicar
    document.getElementById('mainImage').addEventListener('click', () => {
      const img = document.getElementById('mainImage');
      if(img.src.includes('foto-pb.jpg')) {
        img.src = 'https://i.postimg.cc/s11zJ6Sv/foto-colorida.jpg';
      } else {
        img.src = 'https://i.postimg.cc/HVxrX9W2/foto-pb.jpg';
      }
    });

    // Inicialização
    window.onload = () => {
      createStars(50);
      animate();
      setInterval(explodeBigHearts, 5000);
    };
  </script>
</body>
</html>
