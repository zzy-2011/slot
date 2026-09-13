(() => {
  'use strict';
  const cv = document.getElementById('game'); const ctx = cv.getContext('2d');
  const W = cv.width, H = cv.height;
  const dpr = Math.max(1, Math.min(3, window.devicePixelRatio || 1));
  cv.width = W * dpr; cv.height = H * dpr; ctx.scale(dpr, dpr);
  const coinsEl = document.getElementById('coins'), scoreEl = document.getElementById('score');
  const overlay = document.getElementById('overlay'), ovTitle = document.getElementById('ov-title'), ovSub = document.getElementById('ov-sub');
  const SYM = ['🍒', '🍋', '🔔', '⭐', '💎', '7️⃣'];
  const RW = 90, RH = 90, RX0 = (W - RW * 3 - 20 * 2) / 2, RY = 70, RG = 20;
  const BX = W / 2 - 70, BY = 240, BW = 140, BH = 50;
  let reels, stopped, spinning, coins, score, result, resultTimer;

  function reset() { reels = [0, 0, 0]; stopped = [true, true, true]; spinning = false; coins = 100; score = 0; result = ''; coinsEl.textContent = '100'; scoreEl.textContent = '0'; overlay.classList.add('hidden'); }
  function spin() {
    if (spinning) return;
    if (coins < 5) { result = '金币不足！'; return; }
    coins -= 5; coinsEl.textContent = coins; spinning = true; stopped = [false, false, false]; result = '';
    for (let i = 0; i < 3; i++) setTimeout(() => { reels[i] = Math.floor(Math.random() * SYM.length); stopped[i] = true; if (i === 2) finish(); }, 600 + i * 500);
  }
  function finish() {
    spinning = false;
    if (reels[0] === reels[1] && reels[1] === reels[2]) { coins += 50; score += 50; result = '🎉 三连大奖 +50！'; }
    else if (reels[0] === reels[1] || reels[1] === reels[2] || reels[0] === reels[2]) { coins += 10; score += 10; result = '两连 +10'; }
    else result = '再接再厉';
    coinsEl.textContent = coins; scoreEl.textContent = score;
  }
  function draw() {
    ctx.fillStyle = '#1a1c3a'; ctx.fillRect(0, 0, W, H);
    if (spinning) for (let i = 0; i < 3; i++) if (!stopped[i]) reels[i] = Math.floor(Math.random() * SYM.length);
    for (let i = 0; i < 3; i++) {
      const x = RX0 + i * (RW + RG);
      ctx.fillStyle = '#22254a'; ctx.fillRect(x, RY, RW, RH);
      ctx.strokeStyle = '#6c7bff'; ctx.lineWidth = 2; ctx.strokeRect(x, RY, RW, RH);
      ctx.font = '44px serif'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle'; ctx.fillStyle = '#fff';
      ctx.fillText(SYM[reels[i]], x + RW / 2, RY + RH / 2);
    }
    ctx.fillStyle = '#6c7bff'; ctx.fillRect(BX, BY, BW, BH);
    ctx.fillStyle = '#fff'; ctx.font = 'bold 20px sans-serif'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle'; ctx.fillText('抽奖 (-5)', W / 2, BY + BH / 2);
    ctx.fillStyle = '#ffd23f'; ctx.font = '18px sans-serif'; ctx.fillText(result, W / 2, H - 30);
  }
  function clickHandler(e) {
    const rect = cv.getBoundingClientRect(); const px = (e.clientX - rect.left) / rect.width * W, py = (e.clientY - rect.top) / rect.height * H;
    if (px >= BX && px <= BX + BW && py >= BY && py <= BY + BH) spin();
  }
  cv.addEventListener('click', clickHandler);
  cv.addEventListener('touchend', e => { const t = e.changedTouches[0]; clickHandler({ clientX: t.clientX, clientY: t.clientY }); }, { passive: true });
  document.getElementById('new').addEventListener('click', reset);
  document.getElementById('ov-btn').addEventListener('click', reset);
  function loop() { draw(); requestAnimationFrame(loop); }
  reset(); requestAnimationFrame(loop);
})();
