<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>I'm Sorry</title>
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@500;700&family=Playfair+Display:ital@1&family=Lato:wght@300;400&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --env-w: min(360px, 90vw);
  --env-h: calc(var(--env-w) * 0.638);
  --letter-w: min(340px, 92vw);
}

html, body {
  height: 100%;
  min-height: 100vh;
  min-height: 100dvh;
}

body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(145deg, #d6f0ff, #b8e4ff, #9dd6ff);
  font-family: 'Lato', sans-serif;
  -webkit-tap-highlight-color: transparent;
}

body::before {
  content: "";
  position: fixed;
  inset: 0;
  background-image: radial-gradient(circle, rgba(100,195,255,0.28) 1.5px, transparent 1.5px);
  background-size: 38px 38px;
  pointer-events: none;
  z-index: 0;
}

/* ═══════════════════════════
   SCREEN 1 — ENVELOPE
═══════════════════════════ */
#envelope-screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 1;
  padding: 20px;
}
#envelope-screen.hidden { display: none; }

.env-box {
  position: relative;
  width: var(--env-w);
  height: var(--env-h);
  perspective: 900px;
  perspective-origin: 50% 0%;
}

.env-body {
  position: absolute;
  inset: 0;
  background: #7ec8e3;
  border-radius: 8px 8px 14px 14px;
  box-shadow: 0 20px 55px rgba(40,130,180,0.4), 0 6px 18px rgba(40,130,180,0.2);
}
.env-body::before {
  content: "";
  position: absolute;
  inset: 0;
  clip-path: polygon(0 0, 50% 52%, 0 100%);
  background: rgba(30,120,175,0.13);
  border-radius: 8px 0 0 14px;
}
.env-body::after {
  content: "";
  position: absolute;
  inset: 0;
  clip-path: polygon(100% 0, 50% 52%, 100% 100%);
  background: rgba(20,100,155,0.11);
  border-radius: 0 8px 14px 0;
}
.env-v {
  position: absolute;
  inset: 0;
  clip-path: polygon(50% 52%, 100% 100%, 0 100%);
  background: rgba(50,145,200,0.18);
  border-radius: 0 0 14px 14px;
  pointer-events: none;
}

.flap {
  position: absolute;
  inset: 0;
  transform-origin: top center;
  transform: rotateX(0deg);
  transition: transform 0.9s cubic-bezier(0.45, 0, 0.2, 1);
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  pointer-events: none;
}
.flap-face {
  position: absolute;
  inset: 0;
  clip-path: polygon(0% 0%, 100% 0%, 50% 58%);
  background: #5bb8d8;
  border-radius: 8px 8px 0 0;
}
.flap-face::after {
  content: "";
  position: absolute;
  inset: 0;
  clip-path: polygon(0% 0%, 100% 0%, 50% 58%);
  background: linear-gradient(160deg, rgba(255,255,255,0.16), rgba(20,90,140,0.2));
}

/* WAX SEAL */
.seal {
  position: absolute;
  top: 45%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
  cursor: pointer;
  transition: opacity 0.4s, transform 0.4s;
  touch-action: manipulation;
}
.seal-inner { transition: transform 0.2s ease; }
.seal:hover .seal-inner,
.seal:active .seal-inner { transform: scale(1.08); }

.env-box.opening .flap { transform: rotateX(-178deg); }
.env-box.opening .seal {
  opacity: 0;
  transform: translate(-50%, -50%) scale(0.1);
  pointer-events: none;
}

.env-hint {
  margin-top: 22px;
  font-size: 12px;
  letter-spacing: 0.09em;
  color: #3a7fa0;
  font-weight: 300;
  opacity: 0.8;
  text-align: center;
}

/* ═══════════════════════════
   SCREEN 2 — LETTER
═══════════════════════════ */
#letter-screen {
  display: none;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh;
  min-height: 100dvh;
  padding: 16px 0 20px;
  z-index: 1;
  animation: letterAppear 0.55s cubic-bezier(0.34, 1.4, 0.64, 1) both;
}
#letter-screen.visible { display: flex; }

@keyframes letterAppear {
  from { opacity: 0; transform: translateY(30px) scale(0.96); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

.letter-scroll {
  width: var(--letter-w);
  max-height: calc(100dvh - 90px);
  max-height: calc(100vh - 90px);
  overflow-y: auto;
  overflow-x: hidden;
  border-radius: 10px;
  scrollbar-width: thin;
  scrollbar-color: #7ec8e3 transparent;
  -webkit-overflow-scrolling: touch;
}
.letter-scroll::-webkit-scrollbar { width: 4px; }
.letter-scroll::-webkit-scrollbar-thumb { background: #7ec8e3; border-radius: 10px; }
.letter-scroll::-webkit-scrollbar-track { background: transparent; }

.letter-paper {
  position: relative;
  width: 100%;
  background: #f0faff;
  border-radius: 10px;
  box-shadow: 0 16px 50px rgba(0,0,0,0.18);
  padding: 36px 24px 40px;
  background-image: repeating-linear-gradient(
    transparent, transparent 27px,
    #b8dff0 27px, #b8dff0 28px
  );
  background-position: 0 58px;
}

.tape {
  position: absolute;
  width: 56px;
  height: 18px;
  background: rgba(120, 210, 240, 0.75);
  border-radius: 3px;
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.55);
}
.tape-tr { top: -8px;  right: 20px; transform: rotate(4deg); }
.tape-bl { bottom: -8px; left: 14px; transform: rotate(-3deg); }

.deco-tr { position: absolute; top: -22px; right: -2px; pointer-events: none; }
.deco-bl { position: absolute; bottom: -18px; left: -2px; pointer-events: none; }

.ltr-date {
  display: block;
  font-family: 'Lato', sans-serif;
  font-size: 10px;
  letter-spacing: 0.1em;
  color: #5a9ab8;
  font-weight: 300;
  margin-bottom: 10px;
}
.ltr-salut {
  display: block;
  font-family: 'Dancing Script', cursive;
  font-size: clamp(18px, 5vw, 22px);
  color: #1a5e7a;
  margin-bottom: 8px;
  line-height: 1.4;
}
.ltr-body {
  display: block;
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: #1a4a62;
  font-size: clamp(12px, 3.2vw, 13.5px);
  line-height: 28px;
  white-space: pre-wrap;
  word-break: break-word;
  overflow-wrap: break-word;
}
.ltr-proposal {
  display: block;
  font-family: 'Dancing Script', cursive;
  font-size: clamp(18px, 5vw, 22px);
  color: #0077aa;
  font-weight: 700;
  margin-top: 12px;
  line-height: 1.5;
  word-break: break-word;
}
.ltr-sign {
  display: block;
  font-family: 'Dancing Script', cursive;
  font-size: clamp(14px, 4vw, 16px);
  color: #5a9ab8;
  margin-top: 4px;
}

.scroll-hint {
  font-size: 10px;
  color: #5a9ab8;
  letter-spacing: 0.07em;
  font-weight: 300;
  margin-top: 6px;
  opacity: 0.7;
  transition: opacity 0.4s;
  animation: bob 1.6s infinite;
  flex-shrink: 0;
}
@keyframes bob {
  0%,100% { transform: translateY(0); }
  50%      { transform: translateY(3px); }
}

.btn-back {
  margin-top: 12px;
  padding: 10px 28px;
  border: 1.5px solid #42a5d5;
  border-radius: 30px;
  background: transparent;
  color: #0077aa;
  font-family: 'Lato', sans-serif;
  font-size: 12px;
  letter-spacing: 0.1em;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
  flex-shrink: 0;
  touch-action: manipulation;
  -webkit-appearance: none;
}
.btn-back:hover,
.btn-back:active { background: #42a5d5; color: white; }

.falling {
  position: fixed;
  top: -50px;
  pointer-events: none;
  z-index: 999;
  animation: fallDown linear forwards;
}
@keyframes fallDown {
  0%   { transform: translateY(0) rotate(0deg); opacity: 1; }
  85%  { opacity: 1; }
  100% { transform: translateY(110vh) rotate(480deg); opacity: 0; }
}

@media (max-width: 360px) {
  .letter-paper { padding: 30px 16px 34px; }
}
</style>
</head>
<body>

<!-- ═══ SCREEN 1: ENVELOPE ═══ -->
<div id="envelope-screen">
  <div class="env-box" id="envBox">

    <div class="env-body"><div class="env-v"></div></div>
    <div class="flap"><div class="flap-face"></div></div>

    <!-- WAX SEAL -->
    <div class="seal" id="seal">
      <div class="seal-inner">
        <svg width="88" height="88" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <!-- BRIGHT mirror-gold disc: near-white highlight → vivid yellow → deep amber -->
            <radialGradient id="gDisc" cx="35%" cy="28%" r="70%">
              <stop offset="0%"   stop-color="#fffde0"/>
              <stop offset="12%"  stop-color="#ffe44a"/>
              <stop offset="35%"  stop-color="#f0b800"/>
              <stop offset="65%"  stop-color="#c88000"/>
              <stop offset="85%"  stop-color="#a05a00"/>
              <stop offset="100%" stop-color="#6b3800"/>
            </radialGradient>
            <!-- bright drip gradient -->
            <linearGradient id="gDrip" x1="0%" y1="0%" x2="100%" y2="100%">
              <stop offset="0%"   stop-color="#fff0a0"/>
              <stop offset="40%"  stop-color="#f0b800"/>
              <stop offset="100%" stop-color="#7a4200"/>
            </linearGradient>
            <!-- inner ring fill — slightly darker gold pool -->
            <radialGradient id="gInner" cx="42%" cy="35%" r="60%">
              <stop offset="0%"   stop-color="#fff4b0"/>
              <stop offset="30%"  stop-color="#e8a800"/>
              <stop offset="70%"  stop-color="#b87200"/>
              <stop offset="100%" stop-color="#7a4200"/>
            </radialGradient>
            <!-- broad specular flash -->
            <radialGradient id="gFlash" cx="38%" cy="25%" r="45%">
              <stop offset="0%"   stop-color="rgba(255,255,240,0.72)"/>
              <stop offset="50%"  stop-color="rgba(255,240,140,0.20)"/>
              <stop offset="100%" stop-color="rgba(255,200,0,0)"/>
            </radialGradient>
            <filter id="gShadow" x="-20%" y="-20%" width="140%" height="140%">
              <feDropShadow dx="0" dy="4" stdDeviation="5" flood-color="rgba(60,25,0,0.75)"/>
            </filter>
            <!-- subtle emboss for rose -->
            <filter id="roseEmboss" x="-10%" y="-10%" width="120%" height="120%">
              <feGaussianBlur in="SourceAlpha" stdDeviation="0.6" result="blur"/>
              <feOffset dx="0.5" dy="0.8" result="offset"/>
              <feComposite in="SourceGraphic" in2="offset" operator="over"/>
            </filter>
          </defs>

          <!-- ── SCALLOPED WAX EDGE (irregular blob) ── -->
          <!-- 14 drip lobes via ellipses radiating outward -->
          <ellipse cx="50"  cy="6.5" rx="8.5" ry="6"   fill="url(#gDrip)"/>
          <ellipse cx="68"  cy="11"  rx="6"   ry="8.5" fill="url(#gDrip)"/>
          <ellipse cx="82"  cy="23"  rx="5.5" ry="8"   fill="url(#gDrip)"/>
          <ellipse cx="92"  cy="40"  rx="5.5" ry="7.5" fill="url(#gDrip)"/>
          <ellipse cx="92"  cy="60"  rx="5.5" ry="7.5" fill="url(#gDrip)"/>
          <ellipse cx="82"  cy="77"  rx="5.5" ry="8"   fill="url(#gDrip)"/>
          <ellipse cx="68"  cy="89"  rx="6"   ry="8.5" fill="url(#gDrip)"/>
          <ellipse cx="50"  cy="93.5"rx="8.5" ry="6"   fill="url(#gDrip)"/>
          <ellipse cx="32"  cy="89"  rx="6"   ry="8.5" fill="url(#gDrip)"/>
          <ellipse cx="18"  cy="77"  rx="5.5" ry="8"   fill="url(#gDrip)"/>
          <ellipse cx="8"   cy="60"  rx="5.5" ry="7.5" fill="url(#gDrip)"/>
          <ellipse cx="8"   cy="40"  rx="5.5" ry="7.5" fill="url(#gDrip)"/>
          <ellipse cx="18"  cy="23"  rx="5.5" ry="8"   fill="url(#gDrip)"/>
          <ellipse cx="32"  cy="11"  rx="6"   ry="8.5" fill="url(#gDrip)"/>

          <!-- ── MAIN DISC ── -->
          <circle cx="50" cy="50" r="38" fill="url(#gDisc)" filter="url(#gShadow)"/>

          <!-- Rim highlight ring — bright gold line at edge -->
          <circle cx="50" cy="50" r="37.5" fill="none" stroke="rgba(255,240,140,0.7)" stroke-width="1.2"/>

          <!-- Inner inset ring (recessed look) -->
          <circle cx="50" cy="50" r="31"   fill="url(#gInner)"/>
          <circle cx="50" cy="50" r="31.5" fill="none" stroke="rgba(255,255,180,0.55)" stroke-width="1.0"/>
          <circle cx="50" cy="50" r="29.5" fill="none" stroke="rgba(90,45,0,0.30)"     stroke-width="0.7"/>

          <!-- ── EMBOSSED ROSE (centered at 50,48) ── -->
          <g transform="translate(50,48)" filter="url(#roseEmboss)">
            <!-- shadow duplicate (depth) -->
            <g transform="translate(0.8,1.2)" opacity="0.45">
              <!-- outer petals shadow -->
              <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#6b3000" transform="rotate(0)"/>
              <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#6b3000" transform="rotate(72)"/>
              <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#6b3000" transform="rotate(144)"/>
              <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#6b3000" transform="rotate(216)"/>
              <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#6b3000" transform="rotate(288)"/>
            </g>

            <!-- OUTER petals — deep amber shadows = 3D depth -->
            <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#c07800" transform="rotate(0)"/>
            <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#c07800" transform="rotate(72)"/>
            <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#c07800" transform="rotate(144)"/>
            <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#c07800" transform="rotate(216)"/>
            <ellipse cx="0" cy="-15" rx="5.5" ry="8.5" fill="#c07800" transform="rotate(288)"/>
            <!-- outer petal highlight edge -->
            <ellipse cx="0" cy="-15" rx="3"   ry="5"   fill="rgba(255,240,120,0.55)" transform="rotate(0)"/>
            <ellipse cx="0" cy="-15" rx="3"   ry="5"   fill="rgba(255,240,120,0.55)" transform="rotate(72)"/>
            <ellipse cx="0" cy="-15" rx="3"   ry="5"   fill="rgba(255,240,120,0.55)" transform="rotate(144)"/>
            <ellipse cx="0" cy="-15" rx="3"   ry="5"   fill="rgba(255,240,120,0.55)" transform="rotate(216)"/>
            <ellipse cx="0" cy="-15" rx="3"   ry="5"   fill="rgba(255,240,120,0.55)" transform="rotate(288)"/>

            <!-- MIDDLE petals — brighter -->
            <ellipse cx="0" cy="-10" rx="4.5" ry="6.5" fill="#e09000" transform="rotate(36)"/>
            <ellipse cx="0" cy="-10" rx="4.5" ry="6.5" fill="#e09000" transform="rotate(108)"/>
            <ellipse cx="0" cy="-10" rx="4.5" ry="6.5" fill="#e09000" transform="rotate(180)"/>
            <ellipse cx="0" cy="-10" rx="4.5" ry="6.5" fill="#e09000" transform="rotate(252)"/>
            <ellipse cx="0" cy="-10" rx="4.5" ry="6.5" fill="#e09000" transform="rotate(324)"/>
            <ellipse cx="0" cy="-10" rx="2.5" ry="4"   fill="rgba(255,248,160,0.6)" transform="rotate(36)"/>
            <ellipse cx="0" cy="-10" rx="2.5" ry="4"   fill="rgba(255,248,160,0.6)" transform="rotate(108)"/>
            <ellipse cx="0" cy="-10" rx="2.5" ry="4"   fill="rgba(255,248,160,0.6)" transform="rotate(180)"/>
            <ellipse cx="0" cy="-10" rx="2.5" ry="4"   fill="rgba(255,248,160,0.6)" transform="rotate(252)"/>
            <ellipse cx="0" cy="-10" rx="2.5" ry="4"   fill="rgba(255,248,160,0.6)" transform="rotate(324)"/>

            <!-- INNER petals — near-white gold highlight -->
            <ellipse cx="0" cy="-6"  rx="3.2" ry="5"   fill="#f5c400" transform="rotate(18)"/>
            <ellipse cx="0" cy="-6"  rx="3.2" ry="5"   fill="#f5c400" transform="rotate(90)"/>
            <ellipse cx="0" cy="-6"  rx="3.2" ry="5"   fill="#f5c400" transform="rotate(162)"/>
            <ellipse cx="0" cy="-6"  rx="3.2" ry="5"   fill="#f5c400" transform="rotate(234)"/>
            <ellipse cx="0" cy="-6"  rx="3.2" ry="5"   fill="#f5c400" transform="rotate(306)"/>
            <ellipse cx="0" cy="-6"  rx="1.5" ry="2.8" fill="rgba(255,252,200,0.75)" transform="rotate(18)"/>
            <ellipse cx="0" cy="-6"  rx="1.5" ry="2.8" fill="rgba(255,252,200,0.75)" transform="rotate(90)"/>
            <ellipse cx="0" cy="-6"  rx="1.5" ry="2.8" fill="rgba(255,252,200,0.75)" transform="rotate(162)"/>
            <ellipse cx="0" cy="-6"  rx="1.5" ry="2.8" fill="rgba(255,252,200,0.75)" transform="rotate(234)"/>
            <ellipse cx="0" cy="-6"  rx="1.5" ry="2.8" fill="rgba(255,252,200,0.75)" transform="rotate(306)"/>

            <!-- rose bud centre -->
            <circle cx="0" cy="-1" r="4"   fill="#a05800"/>
            <circle cx="0" cy="-1" r="2.4" fill="#f0b000"/>
            <circle cx="0" cy="-2" r="1"   fill="rgba(255,252,180,0.8)"/>

            <!-- stem -->
            <line x1="0" y1="9" x2="0" y2="18" stroke="#7a4200" stroke-width="2" stroke-linecap="round"/>
            <!-- leaves -->
            <ellipse cx="-4" cy="13" rx="4" ry="2.2" fill="#8a5200" transform="rotate(-35 -4 13)"/>
            <ellipse cx=" 4" cy="15" rx="4" ry="2.2" fill="#8a5200" transform="rotate( 35  4 15)"/>
            <!-- leaf highlight -->
            <ellipse cx="-4" cy="13" rx="2" ry="1"   fill="rgba(255,230,100,0.4)" transform="rotate(-35 -4 13)"/>
            <ellipse cx=" 4" cy="15" rx="2" ry="1"   fill="rgba(255,230,100,0.4)" transform="rotate( 35  4 15)"/>
          </g>

          <!-- ── BROAD SPECULAR FLASH (top-left) ── -->
          <ellipse cx="35" cy="30" rx="18" ry="11" fill="url(#gFlash)" transform="rotate(-30 35 30)"/>
          <!-- tiny sharp hotspot -->
          <ellipse cx="31" cy="27" rx="7"  ry="4"  fill="rgba(255,255,250,0.45)" transform="rotate(-30 31 27)"/>
        </svg>
      </div>
    </div>

  </div>
  <p class="env-hint">Tap the wax seal to open 🩵</p>
</div>

<!-- ═══ SCREEN 2: LETTER ═══ -->
<div id="letter-screen">

  <div class="letter-scroll" id="letterScroll">
    <div class="letter-paper">
      <div class="tape tape-tr"></div>
      <div class="tape tape-bl"></div>

      <!-- top-right flower -->
      <svg class="deco-tr" width="44" height="44" viewBox="0 0 48 48">
        <g transform="translate(24,24)">
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#a8dff5"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#c2eafb" transform="rotate(72)"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#a8dff5" transform="rotate(144)"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#c2eafb" transform="rotate(216)"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#a8dff5" transform="rotate(288)"/>
          <circle cx="0" cy="0" r="6.5" fill="#42b8e0"/>
          <circle cx="0" cy="0" r="3"   fill="#d6f4ff"/>
        </g>
      </svg>

      <!-- bottom-left flower -->
      <svg class="deco-bl" width="32" height="32" viewBox="0 0 36 36">
        <g transform="translate(18,18)">
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#c2eafb"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#a8dff5" transform="rotate(72)"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#c2eafb" transform="rotate(144)"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#a8dff5" transform="rotate(216)"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#c2eafb" transform="rotate(288)"/>
          <circle cx="0" cy="0" r="5"   fill="#42b8e0"/>
          <circle cx="0" cy="0" r="2.5" fill="#d6f4ff"/>
        </g>
      </svg>

      <span class="ltr-date">March 1, 2026</span>
      <span class="ltr-salut">To babi,</span>
      <span class="ltr-body">  
      Babi,

babi can we fix this? I really don’t want to lose you.

But before anything else, I want to speak clearly and responsibly — not from panic, not from fear, and not just because I’m scared of losing you. I want to speak from awareness.

I understand now that the core of everything wasn’t about my love for you. It was about how my actions made you feel unconsidered, unsafe, and secondary in moments where you should have felt prioritized. That is something I failed at. And I take full responsibility for that.

You weren’t asking for grand gestures. You weren’t asking for perfection. You were asking to be considered — naturally, instinctively, without having to demand it. And instead, too many times, you had to carry your disappointment silently. That wasn’t fair to you.

I regret every moment you felt unchosen.
I regret every time you had to adjust when I should have been the one adjusting.
I regret the times my explanations came before my empathy.
I regret that my growth often came after damage instead of before it.

You were patient. You gave chances I didn’t even realize you were giving. And I understand why you’re exhausted.

I don’t want to offer empty promises. So here’s what I’m prepared to do differently — not as words, but as structure:

• I will pause before acting in situations that involve other people or decisions that affect us, and actively ask myself, “How does this impact her?”
• I will stop explaining first and start acknowledging first.
• I will not wait to be called out before reflecting.
• I will communicate proactively instead of defensively.
• I will work on emotional regulation so that my fear or discomfort never overrides your safety again.

This isn’t about “trying harder” for a few weeks. This is about changing the pattern that made you feel unsafe in the first place.

I also understand something important: love alone isn’t enough. You already love me. I already love you. The missing piece has been consistent consideration and emotional security. That is what I want to rebuild — carefully, slowly, intentionally.

I’m not asking you to forget what happened. I’m asking if we can rebuild with clearer standards, stronger communication, and real accountability. Not a reset that ignores the past — but a reset that learns from it.

If we try again, it won’t be the same dynamic. It will require boundaries. It will require uncomfortable conversations. It will require measurable change, not emotional intensity.

I believe what we have is worth fighting for — not because I’m afraid to be alone, but because I genuinely see future value in us when done right.

If you truly feel you cannot continue, I will respect that. But if there is even a small space for rebuilding — slowly, intentionally — I am ready to meet you there with maturity and consistency.

Not just words.

With action.

I love you. And I am willing to do this the right way.
      </span>
      <span class="ltr-proposal"> I love you, always have. always will🩵</span>
      <span class="ltr-sign">— from bab, yours always &amp; forever.</span>
    </div>
  </div>

  <p class="scroll-hint" id="scrollHint">↓ scroll to read more</p>
  <button class="btn-back" id="btnBack">↩ Put back in envelope</button>
</div>

<script>
const envScreen  = document.getElementById('envelope-screen');
const letterScr  = document.getElementById('letter-screen');
const envBox     = document.getElementById('envBox');
const seal       = document.getElementById('seal');
const btnBack    = document.getElementById('btnBack');
const scroll     = document.getElementById('letterScroll');
const scrollHint = document.getElementById('scrollHint');

scroll.addEventListener('scroll', () => {
  if (scroll.scrollTop > 20) scrollHint.style.opacity = '0';
}, { passive: true });

seal.addEventListener('click', () => {
  envBox.classList.add('opening');
  setTimeout(() => {
    envScreen.classList.add('hidden');
    letterScr.classList.add('visible');
    scroll.scrollTop = 0;
    scrollHint.style.opacity = '0.7';
    spawnParticles();
  }, 1000);
});

btnBack.addEventListener('click', () => {
  letterScr.classList.remove('visible');
  envBox.classList.remove('opening');
  envScreen.classList.remove('hidden');
});

const ns = "http://www.w3.org/2000/svg";

function makeRose(size) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width",size); svg.setAttribute("height",size);
  svg.setAttribute("viewBox","0 0 40 40");
  const g = document.createElementNS(ns,"g");
  g.setAttribute("transform","translate(20,20)");
  const outerCols = ["#5bc8e8","#42a8d5","#7dd8f0","#a8dff5","#1da0cc"];
  const innerCols = ["#b8ecff","#d6f4ff","#80d8f0","#c2eafb","#e8f8ff"];
  const oc = outerCols[Math.floor(Math.random()*outerCols.length)];
  const ic = innerCols[Math.floor(Math.random()*innerCols.length)];
  for(let i=0;i<5;i++){
    const e=document.createElementNS(ns,"ellipse");
    e.setAttribute("cx","0"); e.setAttribute("cy","-10");
    e.setAttribute("rx","4.5"); e.setAttribute("ry","7");
    e.setAttribute("fill",oc); e.setAttribute("opacity","0.85");
    e.setAttribute("transform",`rotate(${i*72})`);
    g.appendChild(e);
  }
  for(let i=0;i<5;i++){
    const e=document.createElementNS(ns,"ellipse");
    e.setAttribute("cx","0"); e.setAttribute("cy","-6");
    e.setAttribute("rx","3"); e.setAttribute("ry","5");
    e.setAttribute("fill",ic); e.setAttribute("opacity","0.9");
    e.setAttribute("transform",`rotate(${36+i*72})`);
    g.appendChild(e);
  }
  const dot=document.createElementNS(ns,"circle");
  dot.setAttribute("cx","0"); dot.setAttribute("cy","0");
  dot.setAttribute("r","3.5"); dot.setAttribute("fill","#1a90be");
  g.appendChild(dot);
  const stem=document.createElementNS(ns,"line");
  stem.setAttribute("x1","0"); stem.setAttribute("y1","3");
  stem.setAttribute("x2","0"); stem.setAttribute("y2","12");
  stem.setAttribute("stroke","#2a8060"); stem.setAttribute("stroke-width","1.5");
  stem.setAttribute("stroke-linecap","round");
  g.appendChild(stem);
  svg.appendChild(g); return svg;
}

function makeFlower(size) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width",size); svg.setAttribute("height",size);
  svg.setAttribute("viewBox","0 0 40 40");
  const g = document.createElementNS(ns,"g");
  g.setAttribute("transform","translate(20,20)");
  const cols = ["#a8dff5","#c2eafb","#80d0f0","#5bc8e8","#d6f4ff"];
  const c = cols[Math.floor(Math.random()*cols.length)];
  for(let i=0;i<5;i++){
    const e=document.createElementNS(ns,"ellipse");
    e.setAttribute("cx","0"); e.setAttribute("cy","-9");
    e.setAttribute("rx","4"); e.setAttribute("ry","8");
    e.setAttribute("fill",c);
    e.setAttribute("transform",`rotate(${i*72})`);
    g.appendChild(e);
  }
  const dot=document.createElementNS(ns,"circle");
  dot.setAttribute("cx","0"); dot.setAttribute("cy","0");
  dot.setAttribute("r","5"); dot.setAttribute("fill","#1da0cc");
  g.appendChild(dot); svg.appendChild(g); return svg;
}

function spawnParticles(){
  let n=0;
  const iv=setInterval(()=>{
    if(n++>=38){clearInterval(iv);return;}
    const el=document.createElement('div');
    el.className='falling';
    const size=10+Math.floor(Math.random()*18);
    el.appendChild(Math.random()>0.45?makeRose(size):makeFlower(size));
    el.style.left=(Math.random()*100)+'vw';
    const dur=3.5+Math.random()*4;
    el.style.animationDuration=dur+'s';
    el.style.animationDelay=(Math.random()*0.8)+'s';
    document.body.appendChild(el);
    setTimeout(()=>el.remove(),(dur+1.5)*1000);
  },160);
}
</script>
</body>
</html>
