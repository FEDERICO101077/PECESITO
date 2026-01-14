<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Fish in the Ocean</title>
  <style>
    /* ====== Page / Ocean Background ====== */
    html, body {
      height: 100%;
      margin: 0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      overflow: hidden;
    }

    .ocean {
      position: relative;
      width: 100%;
      height: 100%;
      background: linear-gradient(#7fd3ff 0%, #1e88e5 35%, #0b3d91 100%);
    }

    /* Sunlight rays effect */
    .ocean::before {
      content: "";
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at 20% 10%, rgba(255,255,255,0.35), transparent 40%),
                  radial-gradient(circle at 70% 20%, rgba(255,255,255,0.25), transparent 45%);
      pointer-events: none;
      mix-blend-mode: screen;
    }

    /* ====== Bubbles (decor) ====== */
    .bubble {
      position: absolute;
      bottom: -40px;
      width: 14px;
      height: 14px;
      border-radius: 50%;
      border: 2px solid rgba(255,255,255,0.55);
      opacity: 0.8;
      animation: rise linear infinite;
    }

    @keyframes rise {
      from { transform: translateY(0) translateX(0) scale(1); opacity: 0.0; }
      10%  { opacity: 0.8; }
      to   { transform: translateY(-120vh) translateX(-30px) scale(1.4); opacity: 0.0; }
    }

    /* ====== Fish Container (moves across screen) ====== */
    .fish-wrap {
      position: absolute;
      left: -220px;        /* start off-screen */
      top: 45%;
      width: 220px;
      height: 120px;
      animation: swimAcross 12s linear infinite;
      will-change: transform;
    }

    /* The fish bobs up and down while it swims */
    .fish-wrap .bob {
      position: absolute;
      inset: 0;
      animation: bob 1.6s ease-in-out infinite;
      will-change: transform;
    }

    @keyframes swimAcross {
      from { transform: translateX(-240px); }
      to   { transform: translateX(calc(100vw + 260px)); }
    }

    @keyframes bob {
      0%, 100% { transform: translateY(-10px); }
      50%      { transform: translateY(10px); }
    }

    /* ====== Fish Drawing (CSS shapes) ====== */
    .fish {
      position: absolute;
      left: 20px;
      top: 28px;
      width: 160px;
      height: 64px;
      background: linear-gradient(135deg, #ff9a3d, #ff4d6d);
      border-radius: 40px 50px 50px 40px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.18);
    }

    /* Tail */
    .tail {
      position: absolute;
      left: -35px;
      top: 12px;
      width: 42px;
      height: 40px;
      background: linear-gradient(135deg, #ff6a3d, #ff2d55);
      border-radius: 10px;
      transform: rotate(15deg);
      clip-path: polygon(100% 50%, 0 0, 0 100%);
      animation: wag 0.35s ease-in-out infinite;
      transform-origin: right center;
    }

    @keyframes wag {
      0%, 100% { transform: rotate(18deg); }
      50%      { transform: rotate(-18deg); }
    }

    /* Fin */
    .fin {
      position: absolute;
      left: 72px;
      top: 40px;
      width: 34px;
      height: 24px;
      background: rgba(255,255,255,0.35);
      border-radius: 0 0 20px 20px;
      transform: rotate(-20deg);
    }

    /* Eye */
    .eye {
      position: absolute;
      right: 18px;
      top: 18px;
      width: 16px;
      height: 16px;
      background: #fff;
      border-radius: 50%;
      box-shadow: inset 0 0 0 2px rgba(0,0,0,0.08);
    }
    .eye::after {
      content: "";
      position: absolute;
      left: 5px;
      top: 5px;
      width: 6px;
      height: 6px;
      background: #111;
      border-radius: 50%;
    }

    /* Smile */
    .smile {
      position: absolute;
      right: 10px;
      top: 34px;
      width: 18px;
      height: 10px;
      border: 2px solid rgba(0,0,0,0.35);
      border-top: 0;
      border-left: 0;
      border-right: 0;
      border-radius: 0 0 12px 12px;
      transform: rotate(10deg);
      opacity: 0.7;
    }

    /* ====== Speech Bubble ====== */
    .speech {
      position: absolute;
      left: 110px;
      top: -6px;
      background: rgba(255,255,255,0.92);
      color: #0b2a4a;
      padding: 10px 12px;
      border-radius: 16px;
      font-weight: 700;
      font-size: 16px;
      letter-spacing: 0.2px;
      box-shadow: 0 10px 20px rgba(0,0,0,0.15);
      user-select: none;
      white-space: nowrap;
    }
    .speech::after {
      content: "";
      position: absolute;
      left: 18px;
      bottom: -10px;
      width: 18px;
      height: 18px;
      background: rgba(255,255,255,0.92);
      transform: rotate(45deg);
      border-radius: 4px;
    }

    /* Small hint text */
    .hint {
      position: absolute;
      left: 14px;
      bottom: 12px;
      color: rgba(255,255,255,0.9);
      font-size: 14px;
      text-shadow: 0 1px 3px rgba(0,0,0,0.35);
    }

    /* Reduce motion accessibility */
    @media (prefers-reduced-motion: reduce) {
      .fish-wrap, .bob, .tail, .bubble { animation: none !important; }
    }
  </style>
</head>
<body>
  <div class="ocean" id="ocean">
    <div class="fish-wrap" id="fishWrap">
      <div class="bob">
        <div class="speech">I am alive</div>

        <div class="fish">
          <div class="tail"></div>
          <div class="fin"></div>
          <div class="eye"></div>
          <div class="smile"></div>
        </div>
      </div>
    </div>

    <div class="hint">Tip: Refresh to see different bubble positions 🫧</div>
  </div>

  <script>
    // Make random bubbles
    const ocean = document.getElementById("ocean");

    function spawnBubble() {
      const b = document.createElement("div");
      b.className = "bubble";

      // Random left position across the screen
      const left = Math.random() * 100;
      b.style.left = left + "vw";

      // Random size
      const size = 6 + Math.random() * 14;
      b.style.width = size + "px";
      b.style.height = size + "px";

      // Random speed
      const duration = 6 + Math.random() * 6;
      b.style.animationDuration = duration + "s";

      // Random delay (so they don't all rise together)
      b.style.animationDelay = (Math.random() * 2) + "s";

      // Slight random opacity
      b.style.opacity = (0.35 + Math.random() * 0.55).toFixed(2);

      ocean.appendChild(b);

      // Remove after animation ends
      const life = (duration + 3) * 1000;
      setTimeout(() => b.remove(), life);
    }

    // Start with a few, then keep spawning
    for (let i = 0; i < 18; i++) spawnBubble();
    setInterval(spawnBubble, 500);

    // Optional: randomize fish vertical start position a bit each loop
    const fishWrap = document.getElementById("fishWrap");
    let toggle = false;
    setInterval(() => {
      toggle = !toggle;
      const top = 30 + Math.random() * 40; // 30% to 70% viewport height
      fishWrap.style.top = top + "%";
    }, 12000); // matches swimAcross duration
  </script>
</body>
</html>
