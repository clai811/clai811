<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Menjadi Pacarku?</title>
<style>
  /* Reset */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  html, body {
    height: 100%;
    width: 100%;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #ffb6c1, #ffc0cb);
    color: #660033;
    overflow: hidden;
  }

  body {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    user-select: none;
    position: relative;
    padding: 20px;
  }

  h1 {
    font-size: 2.5rem;
    text-align: center;
    margin-bottom: 40px;
    text-shadow: 1px 1px 4px #fff2f7;
  }

  #button-container {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    justify-content: center;
    width: 100%;
    max-width: 320px;
  }

  button {
    flex: 1 1 140px;
    padding: 15px 20px;
    border: none;
    border-radius: 30px;
    font-size: 1.1rem;
    cursor: pointer;
    font-weight: 600;
    box-shadow: 0 5px 15px rgba(102,0,51,0.3);
    transition: background-color 0.3s ease, transform 0.2s ease;
    user-select: none;
    position: relative;
  }

  button:focus {
    outline: none;
    box-shadow: 0 0 10px #ff69b4;
  }

  #yes-btn {
    background-color: #ff69b4;
    color: white;
  }

  #yes-btn:hover {
    background-color: #ff4c9a;
    transform: scale(1.05);
  }

  #no-btn {
    background-color: #ffe4e6;
    color: #aa336a;
    position: absolute;
    top: 60%;
    transition: top 0.3s ease, left 0.3s ease;
  }

  /* Heart container */
  #hearts-container {
    pointer-events: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: visible;
    z-index: 0;
  }

  .heart {
    position: absolute;
    font-size: 16px;
    color: #ff4c9a;
    opacity: 0.8;
    user-select: none;
    will-change: transform;
    animation-timing-function: ease-out;
  }

  @media (max-width: 400px) {
    h1 {
      font-size: 2rem;
    }
    button {
      font-size: 1rem;
      padding: 12px 18px;
    }
  }
</style>
</head>
<body>
  <h1>Apakah kamu mau menjadi pacarku?</h1>
  <div id="button-container">
    <button id="yes-btn" aria-label="Ya, saya bersedia">Ya, saya bersedia</button>
    <button id="no-btn" aria-label="Tidak, tinggalkan saya sendiri">Tidak, tinggalkan saya sendiri</button>
  </div>

  <div id="hearts-container"></div>

<script>
  // Heart animation following cursor
  const heartsContainer = document.getElementById('hearts-container');
  const colors = ['#ff69b4', '#ff85b1', '#ff4c9a', '#ff6ea1', '#ff85b1'];

  function createHeart(x, y) {
    const heart = document.createElement('div');
    heart.className = 'heart';
    heart.textContent = '♥';
    heart.style.color = colors[Math.floor(Math.random() * colors.length)];
    heart.style.left = x + 'px';
    heart.style.top = y + 'px';
    heart.style.fontSize = (10 + Math.random() * 20) + 'px';
    heartsContainer.appendChild(heart);

    let animDuration = 1500 + Math.random() * 1000;
    let start = null;

    function animate(timestamp) {
      if (!start) start = timestamp;
      let progress = timestamp - start;
      let fraction = progress / animDuration;
      if (fraction < 1) {
        heart.style.transform = 'translateY(' + (-30 * fraction) + 'px) scale(' + (1 - fraction*0.5) + ')';
        heart.style.opacity = 1 - fraction;
        requestAnimationFrame(animate);
      } else {
        heart.remove();
      }
    }
    requestAnimationFrame(animate);
  }

  let lastMousePos = { x: 0, y: 0 };
  document.addEventListener('mousemove', e => {
    lastMousePos = { x: e.clientX, y: e.clientY };
    createHeart(e.clientX, e.clientY);
  });

  // NO button evade logic
  const noBtn = document.getElementById('no-btn');
  const yesBtn = document.getElementById('yes-btn');
  const buttonContainer = document.getElementById('button-container');

  function getRandomPosition() {
    const padding = 10;
    const btnWidth = noBtn.offsetWidth;
    const btnHeight = noBtn.offsetHeight;
    const maxX = window.innerWidth - btnWidth - padding;
    const maxY = window.innerHeight - btnHeight - padding - 50; // leave some space on top
    const x = Math.floor(Math.random() * maxX);
    const y = Math.floor((window.innerHeight * 0.4) + Math.random() * (maxY - window.innerHeight * 0.4));
    return { x, y };
  }

  function moveNoButton() {
    let pos = getRandomPosition();
    noBtn.style.left = pos.x + 'px';
    noBtn.style.top = pos.y + 'px';
  }

  noBtn.addEventListener('mouseenter', () => {
    moveNoButton();
  });

  // Optional: Also move it a bit when the mouse gets close (within 100px)
  document.addEventListener('mousemove', e => {
    const noBtnRect = noBtn.getBoundingClientRect();
    const dx = e.clientX - (noBtnRect.left + noBtnRect.width / 2);
    const dy = e.clientY - (noBtnRect.top + noBtnRect.height / 2);
    const distance = Math.sqrt(dx * dx + dy * dy);
    if (distance < 100) {
      moveNoButton();
    }
  });

  // Initialize NO button position randomly on load
  window.onload = () => {
    moveNoButton();
  };

  // YES button click navigates to page2.html
  yesBtn.addEventListener('click', () => {
    window.location.href = 'page2.html';
  });

  // Accessibility: let keyboard users tab in normal order
  noBtn.style.position = 'absolute';
</script>
</body>
</html>
