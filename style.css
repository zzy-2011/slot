(() => {
  'use strict';
  const cv = document.getElementById('game'); const ctx = cv.getContext('2d');
  const W = cv.width, H = cv.height;
  const dpr = Math.max(1, Math.min(3, window.devicePixelRatio || 1));
  cv.width = W * dpr; cv.height = H * dpr; ctx.scale(dpr, dpr);
  const movesEl = document.getElementById('moves'), timeEl = document.getElementById('time');
  const overlay = document.getElementById('overlay'), ovTitle = document.getElementById('ov-title'), ovSub = document.getElementById('ov-sub');
  const N = 4, CELL = W / N;
  let board, blank, moves, over, timer, secs;

  function reset() {
    board = []; for (let i = 1; i < N * N; i++) board.push(i); board.push(0); blank = N * N - 1;
    for (let k = 0; k < 300; k++) { const nb = neighbors(blank); const pick = nb[Math.floor(Math.random() * nb.length)]; [board[blank], board[pick]] = [board[pick], board[blank]]; blank = pick; }
    moves = 0; over = false; secs = 0; movesEl.textContent = '0'; timeEl.textContent = '0'; overlay.classList.add('hidden');
    if (timer) clearInterval(timer); timer = setInterval(() => { if (!over) { secs++; timeEl.textContent = secs; } }, 1000);
  }
  function neighbors(i) { const r = Math.floor(i / N), c = i % N, res = []; if (r > 0) res.push(i - N); if (r < N - 1) res.push(i + N); if (c > 0) res.push(i - 1); if (c < N - 1) res.push(i + 1); return res; }
  function solved() { for (let i = 0; i < N * N - 1; i++) if (board[i] !== i + 1) return false; return true; }
  function clickHandler(e) {
    if (over) return;
    const rect = cv.getBoundingClientRect(); const px = (e.clientX - rect.left) / rect.width * W, py = (e.clientY - rect.top) / rect.height * H;
    const i = Math.floor(py / CELL) * N + Math.floor(px / CELL);
    if (neighbors(blank).includes(i)) { [board[blank], board[i]] = [board[i], board[blank]]; blank = i; moves++; movesEl.textContent = moves; if (solved()) { over = true; if (timer) clearInterval(timer); ovTitle.textContent = '完成！'; ovSub.textContent = '步数 ' + moves + ' · 用时 ' + secs + ' 秒'; overlay.classList.remove('hidden'); } }
  }
  function draw() {
    ctx.fillStyle = '#1a1c3a'; ctx.fillRect(0, 0, W, H);
    for (let i = 0; i < N * N; i++) {
      if (board[i] === 0) continue;
      const r = Math.floor(i / N), c = i % N, x = c * CELL, y = r * CELL;
      ctx.fillStyle = '#6c7bff'; ctx.fillRect(x + 4, y + 4, CELL - 8, CELL - 8);
      ctx.fillStyle = '#fff'; ctx.font = 'bold ' + (CELL * 0.45) + 'px sans-serif'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
      ctx.fillText(board[i], x + CELL / 2, y + CELL / 2);
    }
  }
  cv.addEventListener('click', clickHandler);
  cv.addEventListener('touchend', e => { const t = e.changedTouches[0]; clickHandler({ clientX: t.clientX, clientY: t.clientY }); }, { passive: true });
  document.getElementById('new').addEventListener('click', reset);
  document.getElementById('ov-btn').addEventListener('click', reset);
  function loop() { draw(); requestAnimationFrame(loop); }
  reset(); requestAnimationFrame(loop);
})();
