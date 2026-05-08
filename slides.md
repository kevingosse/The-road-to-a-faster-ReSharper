---
theme: ./theme
title: The Road to a Faster ReSharper
info: |
  ## The Road to a Faster ReSharper
  Kevin Gosse
drawings:
  persist: false
transition: slide-left
---

<div class="title3">
  <img src="/title3.png" class="title3-bg" />
  <div class="sparkle sp1"></div>
  <div class="sparkle sp2"></div>
  <div class="sparkle sp3"></div>
  <div class="sparkle sp4"></div>
  <div class="sparkle sp5"></div>
  <div class="sparkle sp6"></div>
  <div class="sparkle sp7"></div>
  <div class="sparkle sp8"></div>
  <div class="title3-social">
    <div class="title3-social-item"><img src="/twitter.png" class="title3-social-icon" /> @kookiz</div>
    <div class="title3-social-item"><img src="/bluesky.png" class="title3-social-icon" /> @kevingosse.net</div>
  </div>
</div>

<style>
.title3-social {
  position: absolute;
  bottom: 10%;
  left: 64%;
  z-index: 3;
  display: flex;
  gap: 1.5rem;
}
.title3-social-item {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  color: #e8dcc8;
  font-family: 'Alegreya', serif;
  font-size: 0.9rem;
  background: rgba(0,0,0,0.5);
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
}
.title3-social-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
  filter: brightness(1.5);
}
.title3 {
  position: absolute;
  inset: 0;
  margin: -49px -84px;
  z-index: 10;
}
.title3-bg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.sparkle {
  position: absolute;
  z-index: 2;
  width: 3px;
  height: 3px;
  background: #ffd700;
  opacity: 0;
  pointer-events: none;
  image-rendering: pixelated;
  box-shadow:
    /* vertical arm */
     0px -6px 0 #ffd700,
     0px -3px 0 #ffd700,
     0px  3px 0 #ffd700,
     0px  6px 0 #ffd700,
    /* horizontal arm */
    -6px  0px 0 #ffd700,
    -3px  0px 0 #ffd700,
     3px  0px 0 #ffd700,
     6px  0px 0 #ffd700,
    /* diagonal accents */
    -3px -3px 0 #e6a80088,
     3px -3px 0 #e6a80088,
    -3px  3px 0 #e6a80088,
     3px  3px 0 #e6a80088;
  animation: px-sparkle var(--dur) var(--del) steps(4) infinite;
}
.sp1 { top: 34%; left: 12%;  --dur: 3.2s; --del: 0s; }
.sp2 { top: 22%; left: 29%; --dur: 2.8s; --del: 1.5s; }
.sp3 { top: 38%; left: 48%; --dur: 3.5s; --del: 0.8s; }
.sp4 { top: 35%; left: 20%; --dur: 3.0s; --del: 2.2s; }
.sp5 { top: 47%; left: 39%; --dur: 2.6s; --del: 1.0s; }
.sp6 { top: 42%; left: 30%; --dur: 3.4s; --del: 0.4s; }
.sp7 { top: 21%; left: 18%; --dur: 2.9s; --del: 1.8s; }


@keyframes px-sparkle {
  0%        { opacity: 0; transform: scale(0); }
  10%, 15%  { opacity: 1; transform: scale(0.6); }
  25%, 35%  { opacity: 1; transform: scale(1); }
  50%, 55%  { opacity: 0.7; transform: scale(0.6); }
  70%       { opacity: 0; transform: scale(0); }
  100%      { opacity: 0; transform: scale(0); }
}
</style>

---
clicks: 1
---

<Transition name="curtain">
  <div v-if="$nav.currentPage === $page && $clicks >= 1" class="macron-curtain">
    <video src="/macron.mp4" autoplay class="macron-video" />
  </div>
</Transition>

<style>
.macron-curtain {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: black;
  z-index: 50;
}
.macron-video {
  max-height: 100%;
  max-width: 100%;
}
.curtain-enter-active {
  transition: transform 0.8s ease-out;
}
.curtain-enter-from {
  transform: translateY(-100%);
}
</style>

---
layout: section
---

# Out-of-Process

<v-clicks>

- Started in **2020** — move R# backend to a separate process
- Originally motivated by VS being **32-bit** — only **4 GB** of address space, shared with R#
- VS went 64-bit, but OOP is still relevant: upgrade to **.NET Core** → double-digit perf gains for free
- Architectural rewrites take **years**... users need relief **now**

</v-clicks>

---

# My Mission

<v-clicks>

- Hired by JetBrains in **early 2025**
- Improve ReSharper performance **today**, in-process
- Target metric: reduce the number of **uninstallations for performance reasons**

</v-clicks>

---

<div class="scale-grid">
  <div v-click class="scale-box" v-motion
    :initial="{ opacity: 0, scale: 0.8 }"
    :enter="{ opacity: 1, scale: 1 }">
    <div class="scale-num">22 years old</div>
    <div class="scale-sub">First release in 2004</div>
  </div>
  <div v-click class="scale-box" v-motion
    :initial="{ opacity: 0, scale: 0.8 }"
    :enter="{ opacity: 1, scale: 1 }">
    <div class="scale-num">20 million</div>
    <div class="scale-sub">lines of code</div>
  </div>
</div>

<div class="scale-punchline" v-click>
  One fix at a time.
</div>

<style>
.scale-grid {
  display: flex; gap: 3rem; justify-content: center; margin-top: 6rem;
}
.scale-box {
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 16px; padding: 2.5rem 3rem; text-align: center;
  min-width: 260px;
}
.scale-num {
  font-size: 2rem; font-weight: 800; color: var(--jb-blue);
}
.scale-sub {
  font-size: 0.95rem; opacity: 0.6; margin-top: 0.5rem;
}
.scale-punchline {
  font-size: 1.4rem; text-align: center; font-style: italic;
  opacity: 0.7; margin-top: 3rem;
}
</style>

---

# How to investigate performance issues

<v-click>

Three words: **measure, measure, measure**

</v-click>

<v-click>

**NEVER** optimize code based on gut feelings. My guts are wrong about 50% of the time.

</v-click>

<div v-click class="perf-caveat">
(probably, I haven't measured)
</div>

<div v-click class="perf-steps">
  <div class="perf-step">
    <div class="perf-step-num">1</div>
    <div class="perf-step-text">Identify bottlenecks</div>
  </div>
  <div class="perf-step">
    <div class="perf-step-num">2</div>
    <div class="perf-step-text">Implement the fix</div>
  </div>
  <div class="perf-step perf-step-last">
    <div class="perf-step-num">3</div>
    <div class="perf-step-text">Measure the improvement</div>
  </div>
</div>

<v-click>

Use the right tool for the right task

</v-click>

<div v-click class="perf-triforce">
  The .NET profilers triforce: <strong>dotTrace</strong>, <strong>PerfView</strong>, <strong>Superluminal Performance</strong>
</div>

<v-click>

Continuous measurement is a must to prevent performance regressions

</v-click>

<div v-click class="perf-screenshot">
  <img src="/perforator.png" class="perf-img" />
</div>

<style>
.perf-caveat {
  text-align: right; font-style: italic; opacity: 0.5; margin-top: -0.3rem;
  padding-right: 3rem; font-size: 0.85rem;
}
.perf-steps {
  display: flex; gap: 0; margin: 0.8rem 0; justify-content: center;
}
.perf-step {
  display: flex; align-items: center; background: #087CFA;
  color: white; padding: 0.5rem 0.8rem; position: relative;
  clip-path: polygon(0 0, calc(100% - 12px) 0, 100% 50%, calc(100% - 12px) 100%, 0 100%, 12px 50%);
  padding-left: 1.2rem;
}
.perf-step:first-child {
  clip-path: polygon(0 0, calc(100% - 12px) 0, 100% 50%, calc(100% - 12px) 100%, 0 100%);
  padding-left: 0.8rem; border-radius: 4px 0 0 4px;
}
.perf-step-last {
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%, 12px 50%);
  border-radius: 0 4px 4px 0;
}
.perf-step-num {
  font-size: 1.2rem; font-weight: 900; margin-right: 0.5rem; opacity: 0.6;
}
.perf-step-text {
  font-size: 0.8rem; font-weight: 600; white-space: nowrap;
}
.perf-step {
  opacity: 0;
}
.perf-steps:not(.slidev-vclick-hidden) .perf-step {
  animation: step-in 0.4s ease both;
}
.perf-steps:not(.slidev-vclick-hidden) .perf-step:nth-child(2) {
  animation-delay: 0.4s;
}
.perf-steps:not(.slidev-vclick-hidden) .perf-step:nth-child(3) {
  animation-delay: 0.8s;
}
@keyframes step-in {
  from { opacity: 0; transform: translateX(-20px); }
  to { opacity: 1; transform: translateX(0); }
}
.perf-triforce {
  text-align: center; font-size: 1rem;
  padding: 0.7rem 1.2rem; background: var(--jb-bg-soft);
  border: 1px solid var(--jb-border); border-radius: 8px;
  margin-top: 0.5rem;
}
.perf-screenshot {
  display: flex; justify-content: center; margin-top: 0.5rem;
}
.perf-img {
  max-height: 180px; border-radius: 8px; border: 1px solid var(--jb-border);
}
</style>

---

# Prioritizing the Effort

<div class="survey-results">
  <div class="survey-column">
    <div v-click class="survey-card survey-card-left">
      <div class="survey-icon">⏱️</div>
      <div class="survey-label">Startup Time</div>
      <div class="survey-detail">I want to be able to work as soon as possible</div>
    </div>
  </div>
  <div class="survey-column">
    <div v-click class="survey-card survey-card-right">
      <div class="survey-icon">⌨️</div>
      <div class="survey-label">Typing Latency</div>
      <div class="survey-detail">I want my interactions to feel smooth</div>
    </div>
  </div>
</div>

<style>
.survey-results {
  display: flex; gap: 3rem; justify-content: center; margin: 3rem 0;
}
.survey-column {
  display: flex; flex-direction: column; align-items: center;
  flex: 1; max-width: 300px; gap: 1rem;
}
.survey-card {
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 16px; padding: 2rem; text-align: center; width: 100%;
  transition: transform 0.5s ease, opacity 0.5s ease;
}
.survey-card-left { transform: translateX(0); }
.survey-card-right { transform: translateX(0); }
.slidev-vclick-hidden.survey-card-left { transform: translateX(-100px); opacity: 0; }
.slidev-vclick-hidden.survey-card-right { transform: translateX(100px); opacity: 0; }
.survey-icon { font-size: 3rem; margin-bottom: 0.5rem; }
.survey-label { font-size: 1.3rem; font-weight: 700; }
.survey-detail { font-size: 0.9rem; opacity: 0.7; margin-top: 0.5rem; }
.survey-goal {
  font-size: 0.95rem; font-weight: 600; color: #28B8A0;
  text-align: center; font-style: italic;
  border-left: 3px solid #28B8A0; padding-left: 0.8rem;
  text-align: left;
}
</style>

---
clicks: 2
---

# Reframing the startup problem

<div v-click="1">
<div class="rf-legend">
  <span class="rf-legend-item"><span class="rf-legend-swatch rf-red"></span> UI frozen — user can't work</span>
  <span class="rf-legend-item"><span class="rf-legend-swatch rf-green"></span> VS is responsive</span>
</div>
</div>

<div class="rf-section" v-click="1">
  <div class="rf-label rf-label-old">Before</div>
  <div class="rf-timeline">
    <div class="rf-bar">
      <div class="rf-seg rf-red" style="flex: 1.5"></div>
      <div class="rf-seg rf-green-tiny" style="flex: 0.95"></div>
      <div class="rf-seg rf-red" style="flex: 2"></div>
      <div class="rf-seg rf-green-tiny" style="flex: 0.5"></div>
      <div class="rf-seg rf-red" style="flex: 1.5"></div>
      <div class="rf-seg rf-green-tiny" style="flex: 0.45"></div>
      <div class="rf-seg rf-red" style="flex: 0.6"></div>
      <div class="rf-seg rf-green" style="flex: 3"></div>
    </div>
    <div class="rf-features">
      <div class="rf-feat" style="left: 70%">
        <div class="rf-stem"></div>
        <div class="rf-box">🔍 Go to Type</div>
      </div>
      <div class="rf-feat" style="left: 14%">
        <div class="rf-stem"></div>
        <div class="rf-box">🔎 Inspections</div>
      </div>
      <div class="rf-feat" style="left: 40%">
        <div class="rf-stem"></div>
        <div class="rf-box">🛠️ Refactoring</div>
      </div>
    </div>
  </div>
  
</div>

<div class="rf-section" v-click="2">
  <div class="rf-label rf-label-new">Target</div>
  <div class="rf-timeline">
    <div class="rf-bar">
      <div class="rf-seg rf-red" style="flex: 1"></div>
      <div class="rf-seg rf-green" style="flex: 1.8"></div>
      <div class="rf-seg rf-red-tiny" style="flex: 0.2"></div>
      <div class="rf-seg rf-green" style="flex: 2"></div>
      <div class="rf-seg rf-red-tiny" style="flex: 0.15"></div>
      <div class="rf-seg rf-green" style="flex: 1.5"></div>
      <div class="rf-seg rf-red-tiny" style="flex: 0.15"></div>
      <div class="rf-seg rf-green" style="flex: 1.2"></div>
      <div class="rf-seg rf-red-tiny" style="flex: 0.1"></div>
      <div class="rf-seg rf-green" style="flex: 1.9"></div>
    </div>
    <div class="rf-features">
      <div class="rf-feat" style="left: 10%">
        <div class="rf-stem"></div>
        <div class="rf-box">🔍 Go to Type</div>
      </div>
      <div class="rf-feat" style="left: 42%">
        <div class="rf-stem"></div>
        <div class="rf-box">🔎 Inspections</div>
      </div>
      <div class="rf-feat" style="left: 81%">
        <div class="rf-stem"></div>
        <div class="rf-box">🛠️ Refactoring</div>
      </div>
    </div>
  </div>
</div>

<style>
.rf-section { margin: 1.2rem 0; }
.rf-label { font-weight: 700; font-size: 0.95rem; margin-bottom: 0.3rem; }
.rf-label-old { color: #CC0000; }
.rf-label-new { color: #10b981; }
.rf-timeline { position: relative; }
.rf-bar { display: flex; height: 28px; border-radius: 6px; overflow: hidden; border: 2px solid rgba(0,0,0,0.12); }
.rf-seg { min-width: 2px; }
.rf-red { background: #fca5a5; }
.rf-red-tiny { background: #fca5a5; }
.rf-green-tiny { background: #86efac; }
.rf-green { background: #86efac; }
.rf-features { position: relative; height: 60px; }
.rf-feat { position: absolute; top: 0; transform: translateX(-50%); display: flex; flex-direction: column; align-items: center; }
.rf-stem { width: 2px; height: 16px; background: #888; }
.rf-box { background: white; border: 2px solid var(--jb-border); border-radius: 8px; padding: 0.25rem 0.6rem; font-size: 0.75rem; font-weight: 600; white-space: nowrap; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
.rf-caption { font-size: 0.8rem; margin-top: -0.2rem; }
.rf-caption-bad { color: #CC0000; }
.rf-caption-good { color: #10b981; }
.rf-legend { display: flex; gap: 2rem; justify-content: center; margin-bottom: 0.8rem; }
.rf-legend-item { display: flex; align-items: center; gap: 0.4rem; font-size: 0.8rem; color: #555; }
.rf-legend-swatch { width: 16px; height: 12px; border-radius: 3px; display: inline-block; }
.rf-punchline { text-align: center; margin-top: 1.5rem; font-size: 1.15rem; color: var(--jb-blue); font-style: italic; }
</style>

---
hide: true
---

<div class="question-box" v-click>
  <p class="question-text">Is ReSharper actually slower than it <em>needs</em> to be?</p>
  <p class="question-sub">It does a <em>lot</em> of stuff. Maybe "slow" is as fast as it gets?</p>
</div>

<div class="answer-wrapper">
  <div v-click class="answer-box">
    <p class="answer-text">Yes. This thing can be <strong>much</strong> faster.</p>
  </div>
  <img v-click src="/macron.jpg" class="macron-thumb" />
</div>

<style>
.question-box {
  background: var(--jb-bg-soft);
  border: 2px solid var(--jb-border);
  border-radius: 16px;
  padding: 2rem 3rem;
  text-align: center;
  margin: 2rem auto;
  max-width: 600px;
}
.question-text { font-size: 1.4rem; font-weight: 600; margin-bottom: 0.5rem; }
.question-sub { font-size: 1rem; opacity: 0.7; }
.answer-wrapper {
  position: relative;
  margin: 1rem auto;
  max-width: 500px;
}
.answer-box {
  background: #e8f5e9;
  border: 2px solid #28B8A0;
  border-radius: 16px;
  padding: 1.5rem 3rem;
  text-align: center;
}
.answer-text { font-size: 1.3rem; font-weight: 600; color: #28B8A0; }
.macron-thumb {
  position: absolute;
  bottom: -10px;
  right: -40px;
  width: 80px;
  border-radius: 8px;
  transform: translateY(0);
  transition: transform 0.6s cubic-bezier(0.0, 0.0, 0.2, 1);
}
.slidev-vclick-hidden.macron-thumb {
  transform: translateY(200px);
}
</style>

---

# The Three Culprits

<div class="culprit-grid">
  <div v-click class="culprit">
    <div class="culprit-title" :class="{ active: $clicks >= 4 }">JIT</div>
    <div v-click="4" class="highlight-ring"></div>
  </div>
  <div v-click class="culprit">
    <div class="culprit-title">Generics</div>
  </div>
  <div v-click class="culprit">
    <div class="culprit-title">Threading Model</div>
  </div>
</div>

<style>
.culprit-grid {
  display: flex; gap: 2rem; justify-content: center; margin-top: 3rem;
}
.culprit {
  position: relative;
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 16px; padding: 2rem; text-align: center;
  flex: 1; max-width: 250px; display: grid; place-items: center;
}
.culprit-title {
  font-size: 1.4rem; font-weight: 700;
  transition: color 0.4s ease;
}
.culprit-title.active { color: var(--jb-blue); }
.highlight-ring {
  position: absolute;
  inset: -2px;
  border-radius: 16px;
  border: 3px solid var(--jb-blue);
  box-shadow: 0 0 16px rgba(14, 165, 233, 0.45);
  box-sizing: border-box;
  pointer-events: none;
}
</style>

---

# The JIT

<div class="pipeline" v-click="1">
  <div class="pipe-step pipe-delay-1">
    <div class="pipe-label">C# code</div>
  </div>
  <div class="pipe-arrow pipe-delay-2">→</div>
  <div class="pipe-step pipe-delay-2">
    <div class="pipe-label">IL</div>
    <div class="pipe-sub">compilation · Roslyn</div>
  </div>
  <div class="pipe-arrow pipe-delay-3">→</div>
  <div class="pipe-step pipe-delay-3">
    <div class="pipe-label">Machine code</div>
    <div class="pipe-sub">runtime · JIT compiler</div>
  </div>
</div>

<div class="jit-question" v-click="2">
  <p class="jit-question-text">How much CPU time spent on jitting at startup?</p>
  <p class="jit-question-sub">Visual Studio 2022, ReSharper 2025.1, simple console app</p>
</div>

<div v-click="3" class="jit-gauge">
  <div class="gauge-labels"><span>0%</span><span>100%</span></div>
  <div class="gauge-track"><div class="gauge-fill"></div></div>
  <div class="gauge-number">{{ displayPct }}</div>
</div>

<style>
.pipeline {
  display: flex; align-items: center; justify-content: center;
  gap: 1rem; margin-top: 2rem;
}
.pipe-step {
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 12px; padding: 1rem 2rem; text-align: center;
  flex: 1; max-width: 260px; height: 80px;
  display: flex; flex-direction: column; justify-content: center;
}
.pipe-label { font-size: 1.1rem; font-weight: 700; }
.pipe-sub { font-size: 0.75rem; opacity: 0.6; margin-top: 0.3rem; }
.pipe-arrow { font-size: 1.8rem; font-weight: 700; color: var(--jb-blue); }
.pipe-delay-1, .pipe-delay-2, .pipe-delay-3 {
  opacity: 0; animation: pipe-fade-in 0.4s forwards;
}
.pipe-delay-1 { animation-delay: 0s; }
.pipe-delay-2 { animation-delay: 0.4s; }
.pipe-delay-3 { animation-delay: 0.8s; }
.slidev-vclick-hidden .pipe-delay-1,
.slidev-vclick-hidden .pipe-delay-2,
.slidev-vclick-hidden .pipe-delay-3 { animation: none; opacity: 0; }
@keyframes pipe-fade-in { to { opacity: 1; } }
.jit-question { text-align: center; margin-top: 1.5rem; }
.jit-question-text { font-size: 1.2rem; font-weight: 600; }
.jit-question-sub { font-size: 0.85rem; opacity: 0.5; margin-top: 0.3rem; }
.jit-gauge { text-align: center; margin-top: 1rem; }
.gauge-labels {
  display: flex; justify-content: space-between;
  max-width: 500px; margin: 0 auto;
  font-size: 0.85rem; opacity: 0.5;
}
.gauge-track {
  max-width: 500px; margin: 0.3rem auto;
  height: 28px; background: var(--jb-bg-soft);
  border: 2px solid var(--jb-border); border-radius: 14px;
  overflow: hidden;
}
.gauge-fill {
  height: 100%; width: 0%; background: #FF318C; border-radius: 12px;
}
.gauge-number {
  font-size: 4rem; font-weight: 900; color: #FF318C; margin-top: 0.3rem;
  font-variant-numeric: tabular-nums;
}
</style>

<script setup>
import { ref, watch } from 'vue'
import { useSlideContext } from '@slidev/client'

const ctx = useSlideContext()
const displayPct = ref('0%')
let animFrame = 0

watch(() => ctx.$clicks.value, (val) => {
  if (val >= 3) animateCounter()
})

function animateCounter() {
  cancelAnimationFrame(animFrame)
  const duration = 4000
  const start = performance.now()
  function tick(now) {
    const t = Math.min((now - start) / duration, 1)
    const eased = t * t
    const pct = Math.round(eased * 65)
    displayPct.value = pct + '%'
    const fill = document.querySelector('.gauge-fill')
    if (fill) fill.style.width = pct + '%'
    if (t < 1) animFrame = requestAnimationFrame(tick)
  }
  animFrame = requestAnimationFrame(tick)
}
</script>

---

# Ngen

<v-clicks>

- **Native Image Generator** → precompiles .NET assemblies to native code

</v-clicks>

<div v-click class="ngen-however">However:</div>

<div class="ngen-indent">
<v-clicks>

- 🚫 Ngen doesn't like generics
- 🚫 Ngen requires administrator rights

</v-clicks>
</div>

<div v-click class="ngen-oop">OOP uses .NET Core, which solves the problem:</div>

<div class="ngen-indent">
<v-clicks>

- **Tiered JIT**: makes initial jitting much faster
- **Crossgen**: ngen without admin rights

</v-clicks>
</div>

<div v-click class="ngen-summary">
  Hard to set up · not all users benefit · weakened by generics · obsolete with OOP
</div>

<div v-click class="ngen-equals">=</div>

<div v-click class="ngen-verdict">Not great :/</div>

<style>
.ngen-however {
  font-weight: 600; margin-top: 1rem; margin-bottom: 0.3rem;
}
.ngen-oop {
  font-weight: 600; margin-top: 1rem; margin-bottom: 0.3rem;
}
.ngen-indent {
  padding-left: 2rem;
}
.ngen-summary {
  text-align: center; font-size: 0.9rem; opacity: 0.6;
  margin-top: 1.5rem; font-style: italic;
}
.ngen-equals {
  text-align: center; font-size: 1.5rem; font-weight: 700;
  opacity: 0.5; margin-top: 0.5rem;
}
.ngen-verdict {
  text-align: center; font-size: 1.3rem; font-weight: 600;
  margin-top: 0.3rem;
}
</style>

---

# Reframing

<v-clicks>

- <span style="color: var(--jb-blue); font-weight: 700;">Priority:</span>

</v-clicks>

<div class="reframe-indent">
<v-clicks>

- ⏱ Resharper shouldn't get in the way of the user during startup


</v-clicks>
</div>

<div v-click class="reframe-conclusion">

=> The real problem is jitting on the UI thread

</div>

<div v-click class="reframe-question">

What if we could move it to another thread?

</div>

<div v-click class="reframe-newplan">

**New plan:** pre-jitting types in the background before the UI thread needs them

</div>

<style>
.reframe-indent {
  padding-left: 2rem;
}
.reframe-conclusion {
  text-align: center; font-size: 1.4rem; font-weight: 700;
  margin-top: 3rem;
}
.reframe-question {
  text-align: center; font-size: 1.2rem; font-style: italic;
  margin-top: 0.5rem; color: #888;
}
.reframe-newplan {
  margin-top: 2.5rem; font-size: 1.1rem;
  padding: 0.8rem 1.2rem;
  border-left: 4px solid #0ea5e9;
  background: rgba(14, 165, 233, 0.08);
  border-radius: 4px;
}
</style>

---

# Multicore JIT

<v-click>

.NET supports prejitting out of the box:

</v-click>

<v-click>

```csharp
ProfileOptimization.SetProfileRoot(@"C:\MyAppFolder");
ProfileOptimization.StartProfile("Startup.Profile");
```

</v-click>

<v-clicks>

- Records which types are jitted, and in what order
- On the next run, prejits them in a **background thread**

</v-clicks>

<div v-click class="mcjit-however">However:</div>

<div class="mcjit-indent">
<v-clicks>

- 🚫 Prejits types in **reverse order** → still impacts early startup
- 🚫 Limited generics support → only reference-type specializations (`List<string>` ✅, `List<int>` ❌)
- 🚫 Hard limit on the number of assemblies → completely breaks in Visual Studio

</v-clicks>
</div>

<style>
.mcjit-however {
  font-weight: 600; margin-top: 1rem; margin-bottom: 0.3rem;
}
.mcjit-indent {
  padding-left: 2rem;
}
.mcjit-summary {
  text-align: center; font-size: 1.3rem; font-weight: 600;
  margin-top: 1.5rem;
}
</style>

---

# Homemade Prejitting

<v-click>

```csharp {1-6|all}
[ModuleInitializer]
internal static void Initialize()
{
    var thread = new Thread(PrejitWorker);
    thread.IsBackground = true;
    thread.Start();
}

private static void PrejitWorker()
{
    foreach (var method in prejitList)
    {
        RuntimeHelpers._CompileMethod(method.MethodHandle);
    }
}
```

</v-click>

---
---

# Collecting JIT Events

<div class="etw-block">

<span v-click="1">ETW (PerfView, TraceEvent)</span> <span v-click="2">🚫 Some information was missing</span>

</div>

<div v-click="3">

**⇒** Write a custom profiler 🙃

</div>

<div v-click="4">

**Silhouette** — a library to write .NET profilers in C#

</div>

<div v-click="5">

```csharp {all}{lines:false}
protected override HResult JITCompilationFinished(FunctionId functionId, HResult hrStatus, bool fIsSafeToBlock)
{
    if (Environment.CurrentManagedThreadId == 1)
    {
        var stack = WalkStack();

        if (stack.Contains("JetBrains"))
        {
            WriteToFile(functionId, stack);
        }
    }

    return HResult.S_OK;
}
```

</div>

<style>
.etw-block {
  margin-bottom: 0.5rem;
}
.slidev-code { font-size: 0.75rem !important; }
</style>

---

# Curating the Prejit List

<v-clicks>

- <img src="/race2.svg" class="inline-icon" /> The prejit thread must finish before the UI thread calls the methods

</v-clicks>

<div v-click="2" class="tl-section">
  <div class="tl-heading">Visual Studio 2022</div>
  <div class="tl-row">
    <div class="tl-label">UI Thread</div>
    <div class="tl-track">
      <div class="tl-bar bar-load tl-bar-anim" style="left:0%;width:10%;">Load R#</div>
      <div class="tl-bar bar-init tl-bar-anim tl-delay-1" style="left:35%;width:65%;">R# Initialization</div>
    </div>
  </div>
  <div class="tl-connector" v-click="3">
    <div class="tl-connector-line" style="left:10%;"></div>
  </div>
  <div class="tl-row" v-click="3">
    <div class="tl-label">Prejit Thread</div>
    <div class="tl-track tl-track-offset">
      <div class="tl-bar bar-prejit" style="left:10%;width:30%;">Prejitting</div>
    </div>
  </div>
</div>

<div v-click="4" class="tl-section">
  <div class="tl-heading">Visual Studio 2026</div>
  <div class="tl-row">
    <div class="tl-label">UI Thread</div>
    <div class="tl-track">
      <div class="tl-bar bar-load tl-bar-anim" style="left:0%;width:10%;">Load R#</div>
      <div class="tl-bar bar-init tl-bar-anim tl-delay-1" style="left:10%;width:90%;">R# Initialization</div>
    </div>
  </div>
  <div class="tl-connector" v-click="5">
    <div class="tl-connector-line" style="left:10%;"></div>
  </div>
  <div class="tl-row" v-click="5">
    <div class="tl-label">Prejit Thread</div>
    <div class="tl-track tl-track-offset">
      <div class="tl-bar bar-prejit" style="left:10%;width:30%;">Prejitting</div>
    </div>
  </div>
</div>

<style>
.tl-section {
  margin-top: 1.2rem;
  border: 2px solid rgba(0,0,0,0.15);
  border-radius: 8px;
  padding: 0.7rem 1rem;
  background: rgba(0,0,0,0.02);
}
.tl-heading {
  font-weight: 700; font-size: 1rem;
  margin-bottom: 0.6rem; color: #333;
}
.tl-row {
  display: flex; align-items: center;
  gap: 0.6rem;
}
.tl-row + .tl-row {
  margin-top: 0.8rem;
}
.tl-label {
  width: 100px; font-size: 0.75rem; text-align: right;
  color: #555; flex-shrink: 0;
  font-weight: 600;
}
.tl-track {
  flex: 1; position: relative; height: 30px;
  background: rgba(0,0,0,0.06); border-radius: 4px;
}
.tl-bar {
  position: absolute; top: 0; height: 100%; border-radius: 4px;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.65rem; font-weight: 500; color: white;
  white-space: nowrap; overflow: hidden;
}
.bar-load { background: #f59e0b; }
.bar-init { background: var(--jb-blue); }
.bar-prejit { background: #10b981; }
.tl-bar-anim {
  transform-origin: left;
  transition: opacity 0.4s ease, transform 0.4s ease;
}
.slidev-vclick-hidden .tl-bar-anim {
  opacity: 0 !important;
  transform: scaleX(0) !important;
}
.tl-delay-1 { transition-delay: 0.35s; }
.tl-track-offset {
  background: linear-gradient(to right, transparent 10%, rgba(0,0,0,0.06) 10%);
}
.inline-icon {
  display: inline-block; height: 1.2em; vertical-align: middle;
  margin-right: 0.1em;
}
.tl-connector {
  position: relative;
  margin-left: 106px;
  height: 14px;
}
.tl-connector-line {
  position: absolute;
  top: 0; bottom: 0; width: 0;
  border-left: 2px dotted #999;
}
</style>

---

# Curating the List

<div class="curate-flow">
  <div v-click="1" class="curate-box">📋 Full JIT log<br/><span class="curate-sub">~thousands of methods</span></div>
  <div v-click="2" class="curate-arrow">→</div>
  <div v-click="2" class="curate-box highlight">🔍 CPU profiler<br/><span class="curate-sub">identify freezes</span></div>
  <div v-click="3" class="curate-arrow">→</div>
  <div v-click="3" class="curate-box result">✅ Prejit list<br/><span class="curate-sub">curated subset</span></div>
</div>

<div v-click="2" class="dottrace-container dottrace-anim">
  <img src="/dottrace.png" class="dottrace-img" />
</div>

<style>
.curate-flow {
  display: flex; align-items: center; justify-content: center;
  gap: 1rem; margin-top: 1rem;
}
.curate-box {
  padding: 0.8rem 1.2rem; border-radius: 10px;
  text-align: center; font-size: 0.9rem; font-weight: 600;
  background: rgba(0,0,0,0.04); border: 2px solid rgba(0,0,0,0.12);
  color: #333; min-width: 140px;
}
.curate-box .curate-sub {
  font-size: 0.7rem; font-weight: 400; color: #777;
  display: block; margin-top: 0.2rem;
}
.curate-box.highlight {
  background: #FFF3E0; border-color: #FC801D;
}
.curate-box.result {
  background: #E8F5E9; border-color: #10b981;
}
.curate-arrow {
  font-size: 1.8rem; color: #999; font-weight: 300;
}
.dottrace-container {
  margin-top: 1.5rem;
  text-align: center;
  transition: opacity 0.4s ease 0.35s !important;
}
.dottrace-img {
  width: 100%;
  border-radius: 8px;
  border: 1px solid rgba(0,0,0,0.1);
}
</style>

---

# Prejit Results

<div v-click="1" class="results-metric">
  Cumulated freezes &gt; 100 ms during startup
</div>

<div v-click="2" class="results-context">
  Visual Studio 2022 · ReSharper 2025.2 · Simple console app
</div>

<div v-click="3" class="results-chart">
  <div class="bar-group">
    <div class="bar-label">Baseline</div>
    <div class="bar-track">
      <div class="bar-fill bar-baseline">
        <span class="bar-value">7,666 ms</span>
      </div>
    </div>
  </div>
  <div class="bar-group">
    <div class="bar-label">Prejit enabled</div>
    <div class="bar-track">
      <div class="bar-fill bar-prejit-result">
        <span class="bar-value">4,849 ms</span>
      </div>
    </div>
  </div>
</div>

<div v-click="4" class="results-reduction">
  <span class="reduction-number">−36%</span> cumulated freeze time
</div>

<style>
.results-metric {
  text-align: center; font-weight: 600; font-size: 1rem;
  margin: 1.5rem 0 0.3rem; color: #333;
}
.results-context {
  text-align: center; color: #777; font-size: 0.85rem;
  margin-bottom: 2rem;
}
.results-chart {
  max-width: 700px; margin: 0 auto;
  display: flex; flex-direction: column; gap: 1.2rem;
}
.bar-group {
  display: flex; align-items: center; gap: 1rem;
}
.bar-label {
  width: 130px; text-align: right; font-size: 0.85rem;
  font-weight: 600; color: #555; flex-shrink: 0;
}
.bar-track {
  flex: 1; height: 48px; background: rgba(0,0,0,0.04);
  border-radius: 8px; position: relative; overflow: hidden;
}
.bar-fill {
  height: 100%; border-radius: 8px;
  display: flex; align-items: center; justify-content: flex-end;
  padding-right: 1rem;
  transition: width 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.slidev-vclick-hidden .bar-fill {
  width: 0% !important;
}
.bar-baseline {
  width: 100%; background: linear-gradient(135deg, #ef4444, #dc2626);
}
.bar-prejit-result {
  width: 63.2%; background: linear-gradient(135deg, #10b981, #059669);
}
.bar-value {
  font-size: 0.9rem; font-weight: 700; color: white;
  text-shadow: 0 1px 2px rgba(0,0,0,0.2);
}
.results-reduction {
  text-align: center; margin-top: 2rem;
}
.reduction-number {
  font-size: 2.5rem; font-weight: 800;
  color: #10b981;
}
</style>

---
clicks: 1
---

# The Three Culprits

<div class="culprit-stage">
  <div class="culprit-row">
    <div class="culprit">
      <div class="culprit-title" :class="{ active: $clicks === 0 }">JIT</div>
      <div class="highlight-ring" :style="`--pos: ${Math.min($clicks, 1)}`"></div>
    </div>
    <div class="culprit">
      <div class="culprit-title" :class="{ active: $clicks >= 1 }">Generics</div>
    </div>
    <div class="culprit">
      <div class="culprit-title">Threading Model</div>
    </div>
  </div>
</div>

<style>
.culprit-stage {
  display: flex;
  justify-content: center;
  margin-top: 3rem;
}
.culprit-row {
  display: flex; gap: 2rem;
}
.culprit-row .culprit {
  position: relative;
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 16px; padding: 2rem; text-align: center;
  width: 250px;
  display: grid; place-items: center;
  box-sizing: border-box;
}
.culprit-row .culprit:first-child {
  z-index: 10;
}
.culprit-row .culprit-title {
  font-size: 1.4rem; font-weight: 700;
  transition: color 0.6s ease;
}
.culprit-row .culprit-title.active {
  color: var(--jb-blue);
}
.highlight-ring {
  position: absolute;
  inset: -2px;
  border-radius: 16px;
  border: 3px solid var(--jb-blue);
  box-shadow: 0 0 16px rgba(14, 165, 233, 0.45);
  box-sizing: border-box;
  transform: translateX(calc(var(--pos, 0) * (250px + 2rem)));
  transition: transform 0.7s cubic-bezier(0.5, 0, 0.2, 1);
  pointer-events: none;
}
</style>

---
clicks: 4
---

# Generics Everywhere

<div class="ge-stage" style="position: relative; height: 400px;">
  <div class="ge-code" :class="{ clicked: $clicks >= 1 }" style="position: relative; z-index: 1;">
    <pre class="ge-pre"><code><span class="ge-event-line"><span class="kw">public event</span> Action&lt;<span class="bi">string</span>&gt; MyEvent;</span><span class="ge-signal-line">
<span class="kw">public</span> Signal&lt;<span class="bi">string</span>&gt; MyEvent;</span></code></pre>
  </div>

  <svg class="ge-svg" style="position: absolute; inset: 0; z-index: 3; pointer-events: none;" viewBox="0 0 860 400" fill="none">
    <defs>
      <marker id="ge-ah" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
        <polygon points="0 0, 10 3.5, 0 7" fill="#333" />
      </marker>
    </defs>
    <g v-click="2">
      <line x1="150" y1="60" x2="480" y2="120" stroke="#333" stroke-width="2" marker-end="url(#ge-ah)" />
      <text x="340" y="82" fill="#aaa" font-size="4" font-style="italic" font-family="sans-serif" transform="rotate(10.3, 300, 85)">wraps a</text>
    </g>
    <g v-click="3">
      <line x1="580" y1="120" x2="700" y2="220" stroke="#333" stroke-width="2" marker-end="url(#ge-ah)" />
      <text x="600" y="150" fill="#aaa" font-size="4" font-style="italic" font-family="sans-serif" transform="rotate(39.8, 620, 160)">identified by</text>
    </g>
    <g v-click="4">
      <line x1="530" y1="135" x2="370" y2="310" stroke="#333" stroke-width="2" marker-end="url(#ge-ah)" />
      <text x="430" y="230" fill="#aaa" font-size="4" font-style="italic" font-family="sans-serif" transform="rotate(-47.6, 430, 220)">exposes a</text>
    </g>
  </svg>

  <div v-click="2" class="ge-box" style="position: absolute; z-index: 2; left: 480px; top: 100px;"><code>Property&lt;T&gt;</code></div>
  <div v-click="3" class="ge-box" style="position: absolute; z-index: 2; left: 700px; top: 200px;"><code>PropertyId&lt;T&gt;</code></div>
  <div v-click="4" class="ge-box" style="position: absolute; z-index: 2; left: 250px; top: 310px;"><code>PropertyBeforeChangeSignal&lt;T&gt;</code></div>
</div>

<style>
.ge-stage {
  position: relative;
  height: 400px;
}
.ge-pre {
  background: var(--jb-bg-soft);
  border: 1px solid var(--jb-border);
  border-radius: 6px;
  padding: 0.5rem 1rem;
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.85rem;
  margin: 0 0 0.4rem 0;
  width: fit-content;
}
.ge-pre code { color: #333; }
.ge-pre .kw { color: #0033b3; }
.ge-pre .bi { color: #871094; }
.ge-signal-line {
  visibility: hidden;
}
.ge-code.clicked .ge-signal-line {
  visibility: visible;
}
.ge-event-line {
  position: relative;
}
.ge-code.clicked .ge-event-line::after {
  content: '';
  position: absolute;
  left: 0; right: 0;
  top: 50%;
  height: 3px;
  background: #FF318C;
  transform-origin: left;
  animation: ge-strike 0.4s ease-out forwards;
}
@keyframes ge-strike {
  from { transform: scaleX(0); }
  to { transform: scaleX(1); }
}
.ge-box {
  position: absolute;
  background: var(--jb-bg-soft);
  border: 2px solid var(--jb-border);
  border-radius: 10px;
  padding: 0.4rem 0.9rem;
  font-weight: 600;
  white-space: nowrap;
}
.ge-box code {
  color: #333;
  font-size: 0.85rem;
  background: none;
  padding: 0;
}
.ge-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}
</style>

---

# Generic Specialization

<div class="spec-cols">
  <div class="spec-col">
    <div v-click="1" class="spec-label">Value types</div>
    <div class="spec-grid">
      <code v-click="2">Signal&lt;<span class="sc-bi">int</span>&gt;</code>
      <span v-click="2" class="spec-arrow" style="transition-delay: 0.15s;">→</span>
      <div v-click="2" class="spec-asm" style="transition-delay: 0.3s;">⚙ 48 8B 4C 24 08 4C 89 E7 ...</div>
      <code v-click="3">Signal&lt;<span class="sc-bi">long</span>&gt;</code>
      <span v-click="3" class="spec-arrow" style="transition-delay: 0.15s;">→</span>
      <div v-click="3" class="spec-asm" style="transition-delay: 0.3s;">⚙ 55 48 89 E5 48 83 EC 20 ...</div>
      <code v-click="4">Signal&lt;<span class="sc-bi">ulong</span>&gt;</code>
      <span v-click="4" class="spec-arrow" style="transition-delay: 0.15s;">→</span>
      <div v-click="4" class="spec-asm" style="transition-delay: 0.3s;">⚙ 41 57 41 56 53 48 83 EC ...</div>
    </div>
    <div v-click="5" class="spec-drawback" style="margin-top: 1.5rem;">🚫 More work for the JIT</div>
  </div>
  <div class="spec-col">
    <div v-click="6" class="spec-label">Reference types</div>
    <div class="spec-grid">
      <code v-click="7" style="grid-area: 1/1;">Signal&lt;<span class="sc-bi">string</span>&gt;</code>
      <code v-click="7" style="grid-area: 2/1; transition-delay: 0.15s;">Signal&lt;<span class="sc-tp">EventArgs</span>&gt;</code>
      <code v-click="7" style="grid-area: 3/1; transition-delay: 0.3s;">Signal&lt;<span class="sc-tp">Object</span>&gt;</code>
      <span v-click="8" class="spec-arrow" style="grid-area: 1/2;">→</span>
      <span v-click="8" class="spec-arrow" style="grid-area: 2/2; transition-delay: 0.1s;">→</span>
      <span v-click="8" class="spec-arrow" style="grid-area: 3/2; transition-delay: 0.2s;">→</span>
      <code v-click="9" class="spec-canon-label" style="grid-area: 1/3;">Signal&lt;<span class="sc-tp">__Canon</span>&gt;</code>
      <div v-click="8" class="spec-asm" style="grid-area: 2/3; transition-delay: 0.3s;">⚙ 89 5C 24 10 57 48 83 EC ...</div>
    </div>
  </div>
</div>

<style>
.spec-cols {
  display: flex;
  gap: 4rem;
  justify-content: center;
  margin-top: 3rem;
}
.spec-col {
  flex: 1;
  max-width: 380px;
}
.spec-label {
  font-weight: 600;
  font-size: 0.9rem;
  margin-bottom: 0.8rem;
  color: #888;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
.spec-grid {
  display: grid;
  grid-template-columns: auto auto 1fr;
  gap: 0.6rem 0.8rem;
  align-items: center;
}
.spec-grid > code {
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  color: #333;
}
.spec-arrow {
  color: #999;
  font-size: 1.1rem;
}
.spec-asm {
  background: #1e1e1e;
  border-radius: 6px;
  padding: 0.3rem 0.7rem;
  min-width: 100px;
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.55rem;
  color: #6a9955;
  white-space: nowrap;
  letter-spacing: 0.05em;
}
.sc-bi { color: #0033b3; }
.sc-tp { color: #871094; }
.spec-drawback {
  font-size: 0.8rem;
  color: #555;
  margin-top: 0.4rem;
}
.spec-canon-label {
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  font-weight: normal;
  color: #333;
  background: none !important;
  border: none !important;
  padding: 0 !important;
  text-align: center;
  align-self: end;
  margin-bottom: -0.8rem;
}
</style>

---
hide: true
---

# Runtime Resolution

<div v-click="1">
<pre class="rt-pre"><code><span class="kw">public class</span> Property&lt;<span class="tp">TValue</span>&gt;
{
    <span class="kw">public</span> Property(<span class="bi">string</span> id)
      : <span class="kw">this</span>(<span class="kw">new</span> PropertyId&lt;<span class="tp">TValue</span>&gt;(id), ...)
    {
    }
}</code></pre>
</div>

<div class="rt-compare">
  <div class="rt-row">
    <span v-click="2" class="rt-tag rt-tag-val">Value types</span>
    <code v-click="3">Property&lt;<span class="sc-bi">int</span>&gt;</code>
    <span v-click="3" class="rt-needs" style="transition-delay: 0.15s;">needs</span>
    <code v-click="3" style="transition-delay: 0.3s;">PropertyId&lt;<span class="sc-bi">int</span>&gt;</code>
    <span v-click="4" class="rt-arrow">→</span>
    <span v-click="4" class="rt-badge rt-fast" style="transition-delay: 0.15s;">Hardcoded at JIT time</span>
  </div>
  <div class="rt-row">
    <span v-click="5" class="rt-tag rt-tag-ref">Reference types</span>
    <code v-click="6">Property&lt;<span class="sc-tp">__Canon</span>&gt;</code>
    <span v-click="6" class="rt-needs" style="transition-delay: 0.15s;">needs</span>
    <code v-click="6" style="transition-delay: 0.3s;">PropertyId&lt;<span class="sc-bi">string</span>&gt;</code>
    <span v-click="7" class="rt-arrow">→</span>
    <span v-click="7" class="rt-badge rt-slow" style="transition-delay: 0.15s;">Resolved at invocation time</span>
  </div>
</div>

<style>
.rt-pre {
  background: var(--jb-bg-soft);
  border: 1px solid var(--jb-border);
  border-radius: 6px;
  padding: 0.6rem 1rem;
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  margin: 0;
  width: fit-content;
}
.rt-pre code { color: #333; }
.rt-pre .kw { color: #0033b3; }
.rt-pre .bi { color: #871094; }
.rt-pre .tp { color: #871094; }
.rt-compare {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  margin-top: 2rem;
}
.rt-row {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  font-size: 0.9rem;
}
.rt-tag {
  font-size: 0.75rem;
  font-weight: 600;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  min-width: 120px;
  text-align: center;
}
.rt-tag-val { background: #E8F0FE; color: #087CFA; }
.rt-tag-ref { background: #FFF3E0; color: #FC801D; }
.rt-row code {
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  color: #333;
}
.rt-needs {
  font-size: 0.8rem;
  color: #999;
  font-style: italic;
}
.rt-arrow { color: #999; font-size: 1.1rem; }
.rt-badge {
  font-weight: 600;
  font-size: 0.85rem;
  padding: 0.3rem 0.8rem;
  border-radius: 6px;
}
.rt-fast { background: #E8F5E9; color: #10b981; }
.rt-slow { background: #FFF0F5; color: #FF318C; }
</style>

---
clicks: 8
---

# Generic Resolution

<div v-click="1" style="position: relative; width: fit-content;">
<pre class="gr-pre"><code><span class="kw">public class</span> Property&lt;<span class="tp">TValue</span>&gt;
{
    <span class="kw">public</span> Property(<span class="bi">string</span> id)
      : <span class="kw">this</span>(<span class="kw">new</span> PropertyId&lt;<span class="tp">TValue</span>&gt;(id), ...)
    {
    }
}</code></pre>
<div v-click="2" class="gr-highlight"></div>
</div>

<div v-click="3">

- <span v-click="3">`Property<string>`</span><span v-click="4"> → `Property<__Canon>` needs `PropertyId<string>`</span><span v-click="5"> → **resolved at invocation time**</span>

</div>

<div v-click="6">

- Must search across **all shared domain assemblies** <span v-click="7">(~250 in Visual Studio)</span>

</div>

<div v-click="8">

- Protected by a **global lock** → heavy contention across cores

</div>

<style>
.gr-pre {
  background: var(--jb-bg-soft);
  border: 1px solid var(--jb-border);
  border-radius: 6px;
  padding: 0.6rem 1rem;
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  margin: 0 0 1rem;
  width: fit-content;
}
.gr-pre code { color: #333; }
.gr-pre .kw { color: #0033b3; }
.gr-pre .bi { color: #871094; }
.gr-pre .tp { color: #871094; }
.gr-highlight {
  position: absolute;
  border: 2px solid #FF318C;
  border-radius: 4px;
  background: rgba(255, 49, 140, 0.08);
  top: 5rem;
  left: 7.5rem;
  width: 13rem;
  height: 1.4rem;
  pointer-events: none;
}
</style>

---
hide: true
---

# Ngen & Generic Instantiations

<div class="ngi-defs">
  <div v-click="1" class="ngi-asm ngi-asm-def">
    <div class="ngi-asm-header">mscorlib.ni.dll</div>
    <code class="ngi-type-label">List&lt;T&gt;</code>
  </div>
  <div v-click="1" class="ngi-asm ngi-asm-def" style="transition-delay: 0.15s;">
    <div class="ngi-asm-header">System.IO.ni.dll</div>
    <code class="ngi-type-label">Stream</code>
  </div>
</div>

<div class="ngi-assemblies">
  <div v-click="2" class="ngi-asm">
    <div class="ngi-asm-header">App.dll</div>
    <pre class="ngi-code"><code><span class="kw">var</span> myList = <span class="kw">new</span> List&lt;Stream&gt;();</code></pre>
    <div v-click="3" class="ngi-inst">List&lt;Stream&gt;</div>
  </div>
  <div v-click="4" class="ngi-asm">
    <div class="ngi-asm-header">Lib.dll</div>
    <pre class="ngi-code"><code><span class="kw">var</span> streams = <span class="kw">new</span> List&lt;Stream&gt;();</code></pre>
    <div v-click="5" class="ngi-inst">List&lt;Stream&gt;</div>
  </div>
</div>

<div v-click="6" class="ngi-takeaway">
  Instantiations stored in the <strong>consumer</strong> assembly → must search <strong>all</strong> ngen assemblies
</div>

<style>
.ngi-defs {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  margin-bottom: 1.5rem;
}
.ngi-asm-def {
  min-width: 150px;
}
.ngi-type-label {
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  color: #333;
}
.ngi-assemblies {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
}
.ngi-asm {
  background: var(--jb-bg-soft);
  border: 2px solid var(--jb-border);
  border-radius: 10px;
  padding: 1rem 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.ngi-asm-header {
  font-weight: 700;
  font-size: 0.8rem;
  color: #888;
  text-transform: uppercase;
  letter-spacing: 0.03em;
}
.ngi-code {
  background: rgba(0,0,0,0.03);
  border: 1px solid var(--jb-border);
  border-radius: 4px;
  padding: 0.3rem 0.6rem;
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.75rem;
  margin: 0;
  flex: 1;
}
.ngi-code code { color: #333; }
.ngi-code .kw { color: #0033b3; }
.ngi-inst {
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
  font-weight: 600;
  color: #FF318C;
  padding: 0.3rem 0.7rem;
  background: #FFF0F5;
  border: 2px solid #FF318C;
  border-radius: 6px;
  white-space: nowrap;
}
.ngi-takeaway {
  text-align: center;
  font-size: 0.95rem;
  margin-top: 1.5rem;
  padding: 0.8rem;
  border-top: 1px solid var(--jb-border);
}
</style>

---
hide: true
---

# The Bottleneck

<div v-click="1" class="bn-scale">
  <div class="bn-boxes">
    <div class="bn-mini" v-for="i in 175" :key="i"></div>
  </div>
  <div class="bn-count">250+ ngen assemblies in Visual Studio</div>
</div>

<div v-click="2" class="bn-lock">🔒 Global lock</div>

<div v-click="3" class="bn-timeline">
  <div class="bn-thread">
    <span class="bn-tname">Core 1</span>
    <div class="bn-track">
      <div class="s sw" style="flex:10"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:36"></div><div class="s sb" style="flex:26"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:12"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 2</span>
    <div class="bn-track">
      <div class="s sw" style="flex:10"></div><div class="s sb" style="flex:8"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:42"></div><div class="s sb" style="flex:20"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:4"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 3</span>
    <div class="bn-track">
      <div class="s sw" style="flex:14"></div><div class="s sb" style="flex:20"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:58"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 4</span>
    <div class="bn-track">
      <div class="s sw" style="flex:48"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:44"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 5</span>
    <div class="bn-track">
      <div class="s sw" style="flex:10"></div><div class="s sb" style="flex:16"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:66"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 6</span>
    <div class="bn-track">
      <div class="s sw" style="flex:48"></div><div class="s sb" style="flex:8"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:36"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 7</span>
    <div class="bn-track">
      <div class="s sw" style="flex:48"></div><div class="s sb" style="flex:16"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:28"></div>
    </div>
  </div>
  <div class="bn-thread">
    <span class="bn-tname">Core 8</span>
    <div class="bn-track">
      <div class="s sw" style="flex:48"></div><div class="s sb" style="flex:24"></div><div class="s sa" style="flex:8"></div><div class="s sw" style="flex:20"></div>
    </div>
  </div>
  <div class="bn-legend">
    <span class="bn-leg"><span class="bn-dot dot-w"></span>Working</span>
    <span class="bn-leg"><span class="bn-dot dot-a"></span>Resolving</span>
    <span class="bn-leg"><span class="bn-dot dot-b"></span>Waiting on lock</span>
  </div>
</div>


<style>
.bn-scale {
  text-align: center;
  margin-bottom: 0.8rem;
}
.bn-boxes {
  display: flex;
  flex-wrap: wrap;
  gap: 3px;
  justify-content: center;
  max-width: 600px;
  margin: 0 auto 0.5rem;
}
.bn-mini {
  width: 14px;
  height: 14px;
  background: var(--jb-bg-soft);
  border: 1px solid var(--jb-border);
  border-radius: 2px;
}
.bn-count {
  font-size: 0.85rem;
  color: #888;
  font-weight: 600;
}
.bn-lock {
  text-align: center;
  font-size: 1.3rem;
  font-weight: 700;
  margin: 0.6rem 0;
}
.bn-timeline {
  max-width: 600px;
  margin: 0 auto;
}
.bn-thread {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 3px;
}
.bn-tname {
  font-size: 0.65rem;
  font-weight: 600;
  color: #555;
  width: 42px;
  text-align: right;
  flex-shrink: 0;
}
.bn-track {
  flex: 1;
  height: 18px;
  display: flex;
  gap: 1px;
  border-radius: 3px;
  overflow: hidden;
}
.s { min-width: 2px; }
.sw { background: #10b981; }
.sa { background: #087CFA; }
.sb { background: #FF318C44; }
.bn-legend {
  display: flex;
  gap: 1.2rem;
  justify-content: center;
  margin-top: 0.5rem;
}
.bn-leg {
  display: flex;
  align-items: center;
  gap: 0.3rem;
  font-size: 0.65rem;
  color: #666;
}
.bn-dot {
  width: 10px;
  height: 10px;
  border-radius: 2px;
}
.dot-w { background: #10b981; }
.dot-a { background: #087CFA; }
.dot-b { background: #FF318C44; }
</style>

---

# The cost of generic resolution

<div class="gc-question">CPU time spent on resolving generics at startup</div>
<div class="gc-context">
  Visual Studio 2022 · ReSharper 2025.1 · Simple console project
</div>

<div class="gc-gauge">
  <div class="gc-bar-labels"><span>0%</span><span>100%</span></div>
  <div class="gc-track">
    <div class="gc-seg gc-ghc" :style="{ width: $clicks >= 1 ? '10.6%' : '0%' }"></div>
    <div class="gc-seg gc-ghm" :style="{ width: $clicks >= 2 ? '4.3%' : '0%' }"></div>
    <div class="gc-seg gc-jit" :style="{ width: $clicks >= 3 ? '65%' : '0%' }"></div>
  </div>
</div>


<div class="gc-legend">
  <div v-click="1" class="gc-item">
    <span class="gc-dot" style="background: #FC801D;"></span>
    <code>JIT_GenericHandleClass</code>: 10.6%
  </div>
  <div v-click="2" class="gc-item">
    <span class="gc-dot" style="background: #087CFA;"></span>
    <code>JIT_GenericHandleMethod</code>: 4.3%
  </div>
  <div v-click="3" class="gc-item">
    <span class="gc-dot" style="background: #FF318C;"></span>
    JIT: 65%
  </div>
</div>

<div v-click="3" class="gc-total">Generics + JIT = ~80%</div>

<style>
.gc-question {
  text-align: center;
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 0.3rem;
}
.gc-context {
  text-align: center;
  color: #777;
  font-size: 0.85rem;
  margin-bottom: 1.5rem;
}
.gc-gauge {
  text-align: center;
}
.gc-bar-labels {
  display: flex;
  justify-content: space-between;
  max-width: 500px;
  margin: 0 auto;
  font-size: 0.85rem;
  opacity: 0.5;
}
.gc-track {
  max-width: 500px;
  margin: 0.3rem auto;
  height: 28px;
  background: var(--jb-bg-soft);
  border: 2px solid var(--jb-border);
  border-radius: 14px;
  overflow: hidden;
  display: flex;
}
.gc-seg {
  height: 100%;
  width: 0%;
  transition: width 1s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.gc-ghc { background: #FC801D; }
.gc-ghm { background: #087CFA; }
.gc-jit { background: #FF318C; }
.gc-legend {
  max-width: 500px;
  margin: 1.5rem auto 0;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
.gc-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
}
.gc-item code {
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.8rem;
}
.gc-dot {
  width: 12px;
  height: 12px;
  border-radius: 3px;
  flex-shrink: 0;
}
.gc-total {
  text-align: center;
  font-size: 3.5rem;
  font-weight: 900;
  color: #FF318C;
  margin-top: 0.5rem;
  font-variant-numeric: tabular-nums;
}
</style>

---

# Reducing Generics Reliance

No runtime fix — had to **rewrite code** to reduce generic instantiations.

<div class="rg-compare">
<div v-click>

### Before

```csharp
private Action<T>[] mySinks = Array.Empty<Action<T>>();
```

</div>
<div v-click>

### After

```csharp
private Action<T>[] mySinks = null;
```

</div>
</div>

<div v-click class="rg-result">
  <img src="/array_empty.jpg" class="rg-img" />
  <div class="rg-stat"><strong>1 second</strong> saved<br />
5% of all generic resolution cost</div>
</div>

<style>
.rg-compare {
  display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem;
}
.rg-compare h3 { font-size: 0.85rem; opacity: 0.6; margin-bottom: 0.3rem; }
.rg-result {
  display: flex; align-items: center; gap: 1rem;
  margin-top: 1.5rem;

}
.rg-img {
  max-height: 180px; border-radius: 6px;
  border: 1px solid var(--jb-border);
}
.rg-stat { font-size: 0.9rem; }
</style>

---
hide:true

# The Hidden Generic Capture

<div class="hgc-compare">
<div v-click>

### Source

```csharp
class Signal<T>
{
    private Lazy<Lifetime> myField
        = new(() => new Lifetime(), ...);
}
```

</div>
<div v-click>

### Compiler output

```csharp
// Compiler caches delegate in:
class c__DisplayClass0_0<T>
{
    static Func<Lifetime> <>9
        = () => new Lifetime();
}
// New instantiation for every T!
```

</div>
</div>

<div v-click>

```csharp
// Fix: delegate in non-generic class
static class Signal
{
    public static Func<Lifetime> CreateLifetime = () => new Lifetime();
}
class Signal<T>
{
    private Lazy<Lifetime> myField = new(Signal.CreateLifetime, ...);
}
```

</div>

<style>
.hgc-compare {
  display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem;
}
.hgc-compare h3 { font-size: 0.85rem; opacity: 0.6; margin-bottom: 0.3rem; }
</style>

---

# Type Erasure by Hand

<div class="te-compare">
<div v-click>
  <div class="te-label">Before</div>
  <pre class="te-pre"><code><span class="te-tp">Lazy</span>&lt;<span class="te-iface">IReadOnlyList</span>&lt;<span class="te-iface">ILazy</span>&lt;<span class="te-tp">TItem</span>&gt;&gt;&gt;
    <span class="te-fld">myDeps</span>;
<span class="te-tp">Lazy</span>&lt;<span class="te-iface">Tuple</span>&lt;<span class="te-iface">IReadOnlyList</span>&lt;<span class="te-iface">IValueDescriptor</span>&gt;,
    <span class="te-iface">IReadOnlyList</span>&lt;<span class="te-iface">ILazy</span>&lt;<span class="te-tp">TItem</span>&gt;&gt;,
    <span class="te-iface">ComponentProperties</span>&gt;&gt;
    <span class="te-fld">myProps</span>;
<span class="te-tp">Lazy</span>&lt;<span class="te-iface">IJetReadonlyList</span>&lt;<span class="te-iface">ILazy</span>&lt;<span class="te-tp">TItem</span>&gt;&gt;&gt;
    <span class="te-fld">myItems</span>;</code></pre>
</div>
<div v-click>
  <div class="te-label">After</div>
  <pre class="te-pre"><code><span class="te-tp">Lazy</span>&lt;<span class="te-kw">object</span>&gt; <span class="te-fld">myDeps</span>;
<span class="te-tp">Lazy</span>&lt;<span class="te-kw">object</span>&gt; <span class="te-fld">myProps</span>;
<span class="te-tp">Lazy</span>&lt;<span class="te-kw">object</span>&gt; <span class="te-fld">myItems</span>;
&#10;</code></pre>
</div>
</div>

<style>
.te-compare {
  display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem;
}
.te-label { font-size: 0.85rem; opacity: 0.6; margin-bottom: 0.3rem; }
.te-pre {
  background: var(--jb-bg-soft);
  border: 1px solid var(--jb-border);
  border-radius: 6px;
  padding: 0.6rem 1rem;
  font-family: 'Cascadia Mono', monospace;
  font-size: 0.75rem;
  line-height: 1.5;
  margin: 0;
}
.te-pre code { color: #333; }
.te-kw { color: #0033b3; }
.te-iface { color: #871094; }
.te-tp { color: #871094; }
.te-fld { color: #333; }
.te-prop { color: #333; font-style: italic; }
.te-punchline {
  text-align: center; margin-top: 1.5rem;
  font-size: 1.1rem; padding: 1rem;
  border-top: 1px solid var(--jb-border);
}
</style>

---
clicks: 1
---

# The Three Culprits

<div class="culprit-stage">
  <div class="culprit-row">
    <div class="culprit">
      <div class="culprit-title">JIT</div>
      <div class="highlight-ring" :style="`--pos: ${1 + Math.min($clicks, 1)}`"></div>
    </div>
    <div class="culprit">
      <div class="culprit-title" :class="{ active: $clicks === 0 }">Generics</div>
    </div>
    <div class="culprit">
      <div class="culprit-title" :class="{ active: $clicks >= 1 }">Threading Model</div>
    </div>
  </div>
</div>

<style>
.culprit-stage {
  display: flex;
  justify-content: center;
  margin-top: 3rem;
}
.culprit-row {
  display: flex; gap: 2rem;
}
.culprit-row .culprit {
  position: relative;
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 16px; padding: 2rem; text-align: center;
  width: 250px;
  display: grid; place-items: center;
  box-sizing: border-box;
}
.culprit-row .culprit:first-child {
  z-index: 10;
}
.culprit-row .culprit-title {
  font-size: 1.4rem; font-weight: 700;
  transition: color 0.6s ease;
}
.culprit-row .culprit-title.active {
  color: var(--jb-blue);
}
.highlight-ring {
  position: absolute;
  inset: -2px;
  border-radius: 16px;
  border: 3px solid var(--jb-blue);
  box-shadow: 0 0 16px rgba(14, 165, 233, 0.45);
  box-sizing: border-box;
  transform: translateX(calc(var(--pos, 0) * (250px + 2rem)));
  transition: transform 0.7s cubic-bezier(0.5, 0, 0.2, 1);
  pointer-events: none;
}
</style>

---
clicks: 4
hide: true
---

# The Project Model

<div class="pm-grid">
  <div class="pm-comp" :class="{ vis: $clicks >= 1 }">Completion</div>
  <div class="pm-comp" :class="{ vis: $clicks >= 1 }" style="--ci:0.08s">Inspections</div>
  <div class="pm-comp" :class="{ vis: $clicks >= 1 }" style="--ci:0.16s">Refactoring</div>
  <div class="pm-comp" :class="{ vis: $clicks >= 1 }" style="--ci:0.24s">Navigation</div>
  <div class="pm-comp" :class="{ vis: $clicks >= 1 }" style="--ci:0.32s">Model Update</div>
  <div class="pm-acell"><div class="pm-arr pm-r" :class="{ vis: $clicks >= 2, pend: $clicks >= 3, drain: $clicks >= 4 }" style="--dd:0.15s"></div></div>
  <div class="pm-acell"><div class="pm-arr pm-r" :class="{ vis: $clicks >= 2, pend: $clicks >= 3, drain: $clicks >= 4 }" style="--dd:0s"></div></div>
  <div class="pm-acell"><div class="pm-arr pm-r" :class="{ vis: $clicks >= 2, pend: $clicks >= 3, drain: $clicks >= 4 }" style="--dd:0.5s"></div></div>
  <div class="pm-acell"><div class="pm-arr pm-r" :class="{ vis: $clicks >= 2, pend: $clicks >= 3, drain: $clicks >= 4 }" style="--dd:0.3s"></div></div>
  <div class="pm-acell"><div class="pm-arr pm-w" :class="{ vis: $clicks >= 3, glow: $clicks >= 4 }"></div></div>
  <div class="pm-status">
    <span class="pm-st pm-st-ok" :class="{ vis: $clicks === 2 }">Multiple concurrent readers ✅</span>
    <span class="pm-st pm-st-wait" :class="{ vis: $clicks === 3 }">Write requested — waiting for readers ⏳</span>
    <span class="pm-st pm-st-lock" :class="{ vis: $clicks >= 4 }">Exclusive write access 🔒</span>
  </div>
  <div class="pm-model">
    <div class="pm-model-name">Project Model</div>
    <div class="pm-model-desc">Solution · Projects · Source code · References</div>
    <div class="pm-model-lock">🔒 Reader / Writer Lock</div>
  </div>
</div>

<style>
.pm-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: auto 60px auto auto;
  justify-items: center;
  max-width: 680px; margin: 0.5rem auto 0;
  row-gap: 0; column-gap: 0.8rem;
}
.pm-comp {
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 8px; padding: 0.5rem 0.3rem;
  font-size: 0.72rem; font-weight: 600; text-align: center; width: 100%;
  opacity: 0; transform: translateY(-10px);
  transition: opacity 0.4s ease var(--ci, 0s), transform 0.4s ease var(--ci, 0s);
}
.pm-comp.vis { opacity: 1; transform: translateY(0); }
.pm-acell {
  display: flex; justify-content: center; align-items: stretch;
  padding: 4px 0; width: 100%;
}
.pm-arr {
  width: 6px; border-radius: 3px;
  opacity: 0; transform: scaleY(0); transform-origin: top;
  transition: opacity 0.4s ease, transform 0.5s ease;
}
.pm-arr.vis { opacity: 1; transform: scaleY(1); }
.pm-r { background: #10b981; }
.pm-w { background: #FF318C; }
.pm-arr.vis.pend { opacity: 0.45; transition: opacity 0.5s ease; }
.pm-r.drain {
  animation: pm-drain 0.45s ease-out forwards;
  animation-delay: var(--dd, 0s);
}
@keyframes pm-drain {
  from { opacity: 0.45; transform: scaleY(1); }
  to { opacity: 0; transform: scaleY(0); }
}
.pm-w.glow {
  box-shadow: 0 0 14px rgba(255, 49, 140, 0.55);
  width: 10px;
  transition: box-shadow 0.5s ease 0.9s, width 0.3s ease 0.9s, opacity 0.3s ease;
}
.pm-status {
  grid-column: 1 / -1; justify-self: stretch;
  text-align: center; height: 1.6rem;
  position: relative; margin: 0.3rem 0 0.5rem;
}
.pm-st {
  position: absolute; left: 0; right: 0;
  font-size: 0.85rem; font-weight: 600;
  opacity: 0; transition: opacity 0.3s ease;
}
.pm-st.vis { opacity: 1; }
.pm-st-ok { color: #10b981; }
.pm-st-wait { color: #FC801D; }
.pm-st-lock { color: #FF318C; }
.pm-model {
  grid-column: 1 / -1;
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 12px; padding: 1rem 1.5rem;
  text-align: center; width: 100%;
}
.pm-model-name { font-size: 1.2rem; font-weight: 700; }
.pm-model-desc { font-size: 0.8rem; color: #888; margin-top: 0.2rem; }
.pm-model-lock { font-size: 0.75rem; font-weight: 600; color: #555; margin-top: 0.4rem; }
</style>

---

# The problem

<div v-click class="dl-context">
  Most R# features depend on the <strong>Project Model</strong>, protected by a <strong>reader/writer lock</strong>
</div>

<div v-click class="dl-context">
  Visual Studio API = <strong>COM</strong> → can only be called from the <strong>main thread</strong>
</div>

<div class="dl-lanes">
  <div class="dl-lane">
    <div v-click="3" class="dl-hdr">Background Thread</div>
    <div v-click class="dl-box dl-ok">
      Acquire <strong>write lock</strong> on the project model ✅
    </div>
    <div v-click class="dl-box dl-wait">
      Need VS data → <strong>marshal to UI thread</strong> ⏳
    </div>
  </div>
  <div class="dl-lane">
    <div v-click="3" class="dl-hdr">UI Thread</div>
    <div class="dl-spacer"></div>
    <div v-click class="dl-box dl-call">
      VS calls R# — <em>right-click, what goes in the context menu?</em>
    </div>
    <div v-click class="dl-box dl-blocked">
      Need <strong>read lock</strong> → ❌ BLOCKED<br/>
      <span class="dl-sub">Write lock held by background thread!</span>
    </div>
  </div>
</div>

<style>
.dl-context {
  text-align: center; font-size: 0.95rem; margin-bottom: 0.8rem;
  padding: 0.5rem 1rem; background: var(--jb-bg-soft); border-radius: 8px;
  border: 1px solid var(--jb-border);
}
.dl-lanes {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 0.6rem 2rem; max-width: 680px; margin: 0 auto;
}
.dl-lane { display: flex; flex-direction: column; gap: 0.5rem; }
.dl-hdr {
  font-weight: 700; font-size: 0.8rem; text-transform: uppercase;
  letter-spacing: 0.03em; color: #888; text-align: center;
  padding-bottom: 0.3rem; border-bottom: 2px solid var(--jb-border);
}
.dl-spacer { height: 1.8rem; }
.dl-box {
  padding: 0.5rem 0.8rem; border-radius: 8px;
  font-size: 0.8rem; border-left: 3px solid var(--jb-border);
}
.dl-ok { border-left-color: #10b981; background: #f0fdf4; }
.dl-wait { border-left-color: #FC801D; background: #fff7ed; }
.dl-call { border-left-color: #087CFA; background: #eff6ff; }
.dl-blocked { border-left-color: #FF318C; background: #fff0f5; }
.dl-sub { font-size: 0.7rem; color: #888; }
.dl-bang {
  text-align: center; font-size: 2.2rem; font-weight: 900;
  color: #FF318C; margin-top: 0.5rem;
}
</style>

---

# The Fix

<div v-click class="fix-rule">
  The write lock can only be acquired by the <strong>UI thread</strong>
</div>

<div class="fix-outcomes">
  <div v-click class="fix-item fix-pro">
    <span class="fix-icon">✅</span>
    <div>
      <div class="fix-label">Deadlocks: eliminated</div>
      <div class="fix-detail">UI thread always available — no cross-thread dependency</div>
    </div>
  </div>
  <div v-click class="fix-item fix-con">
    <span class="fix-icon">🚫</span>
    <div>
      <div class="fix-label">All project model updates serialized to the UI thread</div>
      <div class="fix-detail">Background threads must marshal writes to the main thread</div>
    </div>
  </div>
</div>

<div v-click class="fix-consequence">
  → Direct cause of <strong>freezes</strong> and <strong>typing latency</strong>
</div>

<style>
.fix-rule {
  text-align: center; font-size: 1.3rem; margin: 1.5rem auto;
  padding: 1.2rem 2rem; max-width: 550px;
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border); border-radius: 12px;
}
.fix-outcomes {
  display: flex; flex-direction: column; gap: 1rem;
  max-width: 600px; margin: 1.5rem auto 0;
}
.fix-item {
  display: flex; align-items: flex-start; gap: 1rem;
  padding: 0.8rem 1rem; border-radius: 10px;
}
.fix-pro { background: #f0fdf4; border: 1px solid #10b981; }
.fix-con { background: #fff0f5; border: 1px solid #FF318C; }
.fix-icon { font-size: 1.5rem; flex-shrink: 0; }
.fix-label { font-weight: 700; font-size: 0.95rem; }
.fix-detail { font-size: 0.8rem; color: #666; margin-top: 0.2rem; }
.fix-consequence {
  text-align: center; font-size: 1.15rem; margin-top: 1.5rem;
  padding: 0.8rem; border-top: 1px solid var(--jb-border);
}
</style>

---

# The Scheduler

<div v-click>

- **OOP** fixes this: R# and VS run in separate threads — write lock no longer blocks the UI
- Changing the threading model would take too much effort

</div>

<div v-click class="sch-key">
  Using the UI thread is fine — as long as it's <strong>not noticeable</strong>
</div>

<div class="sch-modes">
  <div v-click class="sch-mode">
    <span class="sch-lbl">User idle</span>
    <div class="sch-track">
      <div class="s sch-idle" style="flex:5"></div><div class="s sch-rw" style="flex:9"></div><div class="s sch-idle" style="flex:25"></div>
    </div>
    <span class="sch-note">Full speed — use the thread as needed</span>
  </div>
  <div v-click class="sch-mode">
    <span class="sch-lbl">User active</span>
    <div class="sch-track">
      <div class="s sch-ui" style="flex:12"></div><div class="s sch-rw" style="flex:5"></div><div class="s sch-ui" style="flex:8"></div><div class="s sch-rw" style="flex:4"></div><div class="s sch-ui" style="flex:15"></div><div class="s sch-rw" style="flex:5"></div><div class="s sch-ui" style="flex:10"></div><div class="s sch-rw" style="flex:3"></div><div class="s sch-ui" style="flex:12"></div><div class="s sch-rw" style="flex:4"></div><div class="s sch-ui" style="flex:10"></div>
    </div>
    <span class="sch-note">Throttle — squeeze work in <strong>≤ 50 ms</strong> quanta</span>
  </div>
  <div v-click="3" class="sch-legend">
    <span class="sch-leg"><span class="sch-dot sch-drw"></span>R# work</span>
    <span class="sch-leg"><span class="sch-dot sch-dui"></span>User input</span>
  </div>
</div>

<style>
.sch-key {
  text-align: center; font-size: 1.05rem; margin: 0.6rem 0;
  padding: 0.5rem 1.2rem; background: var(--jb-bg-soft);
  border-radius: 8px; border: 1px solid var(--jb-border);
}
.sch-modes {
  display: flex; flex-direction: column; gap: 0.6rem;
  max-width: 700px; margin: 0.8rem auto 0;
}
.sch-mode {
  display: flex; align-items: center; gap: 0.8rem;
}
.sch-lbl {
  font-size: 0.75rem; font-weight: 700; color: #555;
  width: 80px; text-align: right; flex-shrink: 0;
}
.sch-track {
  flex: 1; height: 28px; display: flex;
  gap: 2px; border-radius: 4px; overflow: hidden;
}
.sch-track .s { min-width: 3px; border-radius: 2px; }
.sch-rw { background: #087CFA; }
.sch-ui { background: #10b981; }
.sch-idle { background: #e0e2e6; }
.sch-note {
  font-size: 0.7rem; color: #888; width: 180px; flex-shrink: 0;
}
.sch-legend {
  display: flex; gap: 1.2rem; justify-content: center;
  margin-top: 0.3rem; margin-left: 88px;
}
.sch-leg {
  display: flex; align-items: center; gap: 0.3rem;
  font-size: 0.65rem; color: #666;
}
.sch-dot { width: 10px; height: 10px; border-radius: 2px; }
.sch-drw { background: #087CFA; }
.sch-dui { background: #10b981; }
.sch-bottom {
  text-align: center; font-size: 0.95rem; margin-top: 1rem;
  padding: 0.6rem 1rem; border-top: 1px solid var(--jb-border);
}
</style>

---
clicks: 9
hide: true
---

# Finding the right balance is tricky

<v-clicks>

- Work is split into small quanta · background tasks are **interruptible**
- During startup, every write lock request forces BG tasks to **stop and restart**

</v-clicks>

<div class="path-container">
  <div class="path-row">
    <span class="path-lbl">UI Thread</span>
    <div class="path-track">
      <div class="ps" style="flex:18"></div>
      <div class="ps ps-wait" :class="{ pv: $clicks >= 3 }" style="flex:5"></div>
      <div class="ps ps-work" :class="{ pv: $clicks >= 4 }" style="flex:2"></div>
      <div class="ps" style="flex:19"></div>
      <div class="ps ps-wait" :class="{ pv: $clicks >= 5 }" style="flex:5"></div>
      <div class="ps ps-work" :class="{ pv: $clicks >= 6 }" style="flex:2"></div>
      <div class="ps" style="flex:46"></div>
    </div>
  </div>
  <div class="path-row">
    <span class="path-lbl">BG Threads</span>
    <div class="path-track">
      <div class="ps ps-bgw" :class="{ pv: $clicks >= 3 }" style="flex:27"></div>
      <div class="ps" style="flex:2"></div>
      <div class="ps ps-bgw ps-restart" :class="{ pv: $clicks >= 5 }" style="flex:28"><span class="restart-icon">🔄</span></div>
      <div class="ps" style="flex:2"></div>
      <div class="ps ps-bgw ps-restart" :class="{ pv: $clicks >= 7 }" style="flex:22"><span class="restart-icon">🔄</span></div>
      <div class="ps" style="flex:21"></div>
    </div>
  </div>
</div>

<div class="path-legend">
  <span class="path-leg"><span class="path-dot pd-wait"></span>Waiting for BG to stop</span>
  <span class="path-leg"><span class="path-dot pd-work"></span>Actual work</span>
  <span class="path-leg"><span class="path-dot pd-bgw"></span>BG working</span>
</div>

<img :class="{ pv: $clicks >= 9 }" src="/scheduling.png" class="path-screenshot" />

<style>
.path-container {
  max-width: 700px; margin: 1rem auto 0;
}
.path-row {
  display: flex; align-items: center; gap: 0.5rem;
  margin-bottom: 0.5rem;
}
.path-lbl {
  font-size: 0.7rem; font-weight: 700; color: #555;
  width: 80px; text-align: right; flex-shrink: 0;
}
.path-track {
  flex: 1; height: 28px; display: flex;
  gap: 2px; border-radius: 4px;
  background: #f0f1f3;
  position: relative; overflow: visible;
}
.path-track::after {
  content: '';
  position: absolute; right: -1px; top: -1px; bottom: -1px;
  width: 40%;
  background: linear-gradient(to right, transparent 0%, white 40%);
  pointer-events: none;
  z-index: 1;
}
.ps { min-width: 2px; border-radius: 2px; }
.ps-bgw, .ps-wait, .ps-work {
  opacity: 0;
  transition: opacity 0.4s ease;
}
.ps.pv { opacity: 1; }
.ps-wait { background: #FC801D; }
.ps-work { background: #10b981; }
.ps-bgw { background: #087CFA; }
.ps-restart {
  display: flex; align-items: center;
}
.restart-icon {
  font-size: 0.75rem; margin-left: 2px;
  position: relative; left: -1px;
}
.path-legend {
  display: flex; gap: 1rem; justify-content: center;
  margin-top: 0.5rem; padding-left: 80px;
}
.path-leg {
  display: flex; align-items: center; gap: 0.3rem;
  font-size: 0.6rem; color: #666;
}
.path-dot { width: 10px; height: 10px; border-radius: 2px; }
.pd-wait { background: #FC801D; }
.pd-work { background: #10b981; }
.pd-bgw { background: #087CFA; }
.path-screenshot {
  position: absolute; left: 0; right: 0; bottom: 0;
  top: 6.5rem;
  width: 100%;
  object-fit: contain;
  background: white;
  z-index: 10;
  opacity: 0;
  transition: opacity 0.4s ease;
}
.path-screenshot.pv { opacity: 1; }
</style>

---

# Measuring Responsiveness

<ul>
  <li v-click="1">We needed to:
    <ul>
      <li v-click="2">🔍 Identify workloads that need to be split</li>
      <li v-click="3">⚙️ Tune the scheduling algorithm</li>
    </ul>
  </li>
</ul>

<div class="mr-punchline">

<div v-click="4">

**⇒** Write a custom profiler 🙃

</div>

<div v-click="5" class="mr-silhouette">
  <img src="/github.svg" class="mr-social-icon" />kevingosse/silhouette
</div>

</div>

<div v-click="6" class="mr-fps-section">
  <img src="/fps.png" class="mr-fps-img" />
</div>

<style>
:deep(ul ul) {
  padding-left: 1.5rem;
}

.mr-social-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
  filter: invert(1);
}

.mr-punchline {
  margin-top: 1rem; font-size: 1.1rem;
  display: flex; align-items: center; gap: 1.2rem;
}
.mr-silhouette {
  display: flex; align-items: center; gap: 0.4rem;
  font-size: 0.95rem;
}
.mr-fps-section {
  margin-top: 1.2rem; text-align: center;
  display: flex; justify-content: center;
}
.mr-fps-img {
  max-height: 240px; border-radius: 8px;
  border: 1px solid rgba(0,0,0,0.1);
  margin-bottom: 1rem;
}
</style>

---
hide: true
---

# Measure the Time to Process Input

<div class="sm-approach" v-click="1">
  <code>SendMessage(hwnd, WM_NULL, ...)</code>
  <div class="sm-sub">Posts a message and blocks until it's processed</div>
</div>

<div class="sm-fail" v-click="2">🚫 Doesn't work</div>

<div class="sm-diagram" v-click="3">
  <div class="sm-queue-title">Message queue priority</div>
  <div class="sm-queue">
    <div class="sm-msg sm-high"><strong>SendMessage</strong> <span class="sm-note">(processed immediately)</span></div>
    <div class="sm-msg sm-high">Posted messages</div>
    <div class="sm-msg sm-low">Input (keyboard, mouse)</div>
    <div class="sm-msg sm-mid">WM_PAINT</div>
    <div class="sm-msg sm-mid">WM_TIMER</div>
  </div>
</div>

<!--
BONUS: animate messages being dequeued from top, showing input starving at the bottom
-->

<style>
.sm-approach {
  font-size: 0.95rem;
}
.sm-approach code {
  font-size: 1rem; background: #f3f4f6; padding: 0.2rem 0.5rem; border-radius: 4px;
}
.sm-sub { font-size: 0.8rem; color: #666; margin-top: 0.2rem; }
.sm-fail {
  margin-top: 0.8rem; font-size: 1.1rem; font-weight: 700;
}
.sm-diagram {
  margin-top: 0.8rem; max-width: 450px;
}
.sm-queue-title {
  font-size: 0.75rem; color: #888; text-transform: uppercase;
  letter-spacing: 0.03em; margin-bottom: 0.3rem; font-weight: 600;
}
.sm-queue {
  display: flex; flex-direction: column; gap: 2px;
}
.sm-msg {
  padding: 0.3rem 0.8rem; border-radius: 4px;
  font-size: 0.8rem; display: flex; align-items: center; gap: 0.5rem;
}
.sm-note { font-size: 0.7rem; color: #888; }
.sm-high { background: #fee2e2; border-left: 3px solid #ef4444; }
.sm-mid { background: #fef3c7; border-left: 3px solid #f59e0b; }
.sm-low { background: #f0fdf4; border-left: 3px solid #10b981; }
</style>

---
hide: true
---

# SetWindowsHookEx

<div class="hw-approach" v-click="1">
  <code>SetWindowsHookEx(WH_GETMESSAGE, ...)</code>
  <div class="hw-sub">Hook fires when the window processes an <strong>input message</strong></div>
</div>

<div v-click="2" class="hw-problem">
  ⚠️ No user input → no measurement
</div>

<div v-click="3" class="hw-solution-title">Synthetic input</div>

<div class="hw-attempts">
  <div v-click="4" class="hw-attempt hw-bad">
    <div class="hw-label">⌨️ Keyboard</div>
    <div class="hw-issues">
      <div v-click="4">➡️ Use keys that are not mapped to anything</div>
      <div v-click="5">🚫 Breaks two-key shortcuts <span class="hw-ex">ctrl+k, ctrl+c</span></div>
      <div v-click="6">🚫 VS delays extension loading during keyboard input</div>
    </div>
  </div>
  <div v-click="7" class="hw-attempt hw-good">
    <div class="hw-label">🖱️ Mouse</div>
    <div class="hw-issues">
      <div>➡️ Imperceptible movement (±1 pixel)</div>
    </div>
  </div>
</div>

<style>
.hw-approach {
  font-size: 0.95rem;
}
.hw-approach code {
  font-size: 1rem; background: #f3f4f6; padding: 0.2rem 0.5rem; border-radius: 4px;
}
.hw-sub { font-size: 0.85rem; color: #555; margin-top: 0.2rem; }
.hw-problem {
  margin-top: 0.7rem; font-size: 0.95rem;
  padding: 0.4rem 1rem; background: #fef3c7;
  border-radius: 8px; border: 1px solid #f59e0b;
}
.hw-solution-title {
  margin-top: 0.7rem; font-size: 0.9rem; font-weight: 700;
  color: #555; text-transform: uppercase; letter-spacing: 0.03em;
}
.hw-attempts {
  display: flex; flex-direction: column; gap: 0.5rem; margin-top: 0.4rem;
}
.hw-attempt {
  padding: 0.5rem 0.8rem; border-radius: 8px; font-size: 0.85rem;
}
.hw-bad { background: #fef2f2; border: 1px solid #fca5a5; }
.hw-good { background: #f0fdf4; border: 1px solid #86efac; }
.hw-label { font-weight: 700; margin-bottom: 0.3rem; }
.hw-issues { display: flex; flex-direction: column; gap: 0.15rem; font-size: 0.8rem; }
.hw-ex { font-size: 0.75rem; color: #888; }
</style>

---
clicks: 1
---

<div v-if="$clicks >= 1" class="rc-fullscreen">
  <video src="/RightClick.mp4" autoplay loop class="rc-video" />
</div>

<style>
.rc-fullscreen {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: black;
  z-index: 50;
}
.rc-video {
  max-height: 100%;
  max-width: 100%;
}
</style>

---
clicks: 1
---

# The Three Culprits

<div class="culprit-stage">
  <div class="culprit-row">
    <div class="culprit">
      <div class="culprit-title">JIT</div>
      <div class="highlight-ring" :style="`--pos: 2`" :class="{ gone: $clicks >= 1 }"></div>
    </div>
    <div class="culprit">
      <div class="culprit-title">Generics</div>
    </div>
    <div class="culprit">
      <div class="culprit-title" :class="{ active: $clicks === 0 }">Threading Model</div>
    </div>
  </div>
</div>

<div v-if="$clicks >= 1" class="many-more">
  <em>And many, many more...</em>
</div>

<style>
.culprit-stage {
  display: flex;
  justify-content: center;
  margin-top: 3rem;
}
.culprit-row {
  display: flex; gap: 2rem;
}
.culprit-row .culprit {
  position: relative;
  background: var(--jb-bg-soft); border: 2px solid var(--jb-border);
  border-radius: 16px; padding: 2rem; text-align: center;
  width: 250px;
  display: grid; place-items: center;
  box-sizing: border-box;
}
.culprit-row .culprit:first-child {
  z-index: 10;
}
.culprit-row .culprit-title {
  font-size: 1.4rem; font-weight: 700;
  transition: color 0.6s ease;
}
.culprit-row .culprit-title.active {
  color: var(--jb-blue);
}
.highlight-ring {
  position: absolute;
  inset: -2px;
  border-radius: 16px;
  border: 3px solid var(--jb-blue);
  box-shadow: 0 0 16px rgba(14, 165, 233, 0.45);
  box-sizing: border-box;
  transform: translateX(calc(var(--pos, 0) * (250px + 2rem)));
  transition: transform 0.7s cubic-bezier(0.5, 0, 0.2, 1), opacity 0.6s ease;
  pointer-events: none;
}
.highlight-ring.gone {
  opacity: 0;
}
.many-more {
  text-align: center;
  margin-top: 2.5rem;
  font-size: 1.8rem;
  color: #666;
  animation: fade-in 0.8s ease both;
}
@keyframes fade-in {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>

---
transition: none
hide: true
---

# Use the right tool for measuring time

````md magic-move
```csharp
var timerCutOff = DateTime.Now.Ticks + TickThreshold;

do
{
    Run();
}
while (HasAnythingToDo() && DateTime.Now.Ticks < timerCutOff);
```

```csharp
var timerCutOff = DateTime.Now.Ticks + TickThreshold;

do
{
    Run();
}
while (HasAnythingToDo() && DateTime.Now.Ticks < timerCutOff);
```

```csharp
var timerCutOff = DateTime.UtcNow.Ticks + TickThreshold;

do
{
    Run();
}
while (HasAnythingToDo() && DateTime.UtcNow.Ticks < timerCutOff);
```

```csharp
var timerCutOff = DateTime.UtcNow.Ticks + TickThreshold;

do
{
    Run();
}
while (HasAnythingToDo() && DateTime.UtcNow.Ticks < timerCutOff);
```

```csharp
var timer = Stopwatch.StartNew();

do
{
    Run();
}
while (HasAnythingToDo() && timer.ElapsedMilliseconds < ThresholdMs);
```

```csharp
var timer = Stopwatch.StartNew();

do
{
    Run();
}
while (HasAnythingToDo() && timer.ElapsedMilliseconds < ThresholdMs);
```

```csharp
var threshold = Environment.TickCount + ThresholdMs;

do
{
    Run();
}
while (HasAnythingToDo() && Environment.TickCount < threshold);
```

```csharp
var threshold = Environment.TickCount + ThresholdMs;

do
{
    Run();
}
while (HasAnythingToDo() && Environment.TickCount < threshold);
```
````

<div class="list-stack">
  <div v-click="[1, 4]" class="list">
    <div class="list-label">
      <span class="label-swap">
        <span v-show="$slidev.nav.clicks >= 1 && $slidev.nav.clicks < 2">DateTime.Now:</span>
        <span v-show="$slidev.nav.clicks >= 2">DateTime.UtcNow:</span>
      </span>
    </div>
    <ul>
      <li>Slow</li>
      <li>
        <span class="dst-content" :class="{ 'dst-struck': $slidev.nav.clicks >= 3 }">
          <span class="dst-text">Breaks on DST</span>
          <span class="dst-detail">
            <span class="dst-arrow">⇒</span>
            <span class="dst-cases">
              <span>+1 hour: timeout fires early</span>
              <span>&minus;1 hour: timeout fires 1 hour late</span>
            </span>
          </span>
        </span>
      </li>
    </ul>
  </div>

  <div v-click="[4, 6]" class="list">
    <div class="list-label">Stopwatch:</div>
    <ul>
      <li v-click="5">Fast</li>
      <li v-click="5">Submillisecond precision</li>
    </ul>
  </div>

  <div v-click="6" class="list">
    <div class="list-label">Environment.TickCount:</div>
    <ul>
      <li v-click="7">Counts the number of milliseconds of uptime</li>    
      <li v-click="7">Extremely fast</li>
      <li v-click="7">Precision similar to DateTime</li>
    </ul>
  </div>
</div>

<style>
.list-stack {
  display: grid;
}
.list-stack > * {
  grid-row: 1;
  grid-column: 1;
}
.label-swap {
  display: grid;
}
.label-swap > span {
  grid-row: 1;
  grid-column: 1;
}
/* Override v-click animation for label swap: opacity only, no movement */
.label-swap .slidev-vclick-target {
  transition: opacity 0.3s ease !important;
}
.label-swap .slidev-vclick-hidden {
  transform: none !important;
  opacity: 0 !important;
}
.dst-content {
  position: relative;
}
.dst-content::after {
  content: '';
  position: absolute;
  left: 0;
  top: 0.6em;
  width: 0;
  height: 2px;
  background: var(--jb-text-soft);
  transition: width 0.8s ease-out;
}
.dst-content.dst-struck::after {
  width: 100%;
}
.dst-content.dst-struck {
  color: var(--jb-text-soft);
  transition: color 0.6s ease-out 0.3s;
}
.dst-content.dst-struck .dst-arrow {
  color: var(--jb-text-soft);
  transition: color 0.6s ease-out 0.3s;
}
/* Second strikethrough line for the -1 hour row */
.dst-cases span:last-child {
  position: relative;
}
.dst-cases span:last-child::after {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  width: 0;
  height: 2px;
  background: var(--jb-text-soft);
  transition: width 0.6s ease-out 0.5s;
}
.dst-content.dst-struck .dst-cases span:last-child::after {
  width: 100%;
}
.dst-detail {
  display: inline-flex;
  align-items: baseline;
  gap: 0.5rem;
  margin-left: 0.5rem;
}
.dst-arrow {
  color: var(--jb-teal);
  font-size: 1.3rem;
  line-height: 1.4;
}
.dst-cases {
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
}
</style>

---
hide: true
---

# ⚠️ WARNING ⚠️

<div v-click>
  <img src="/issue-51935.png" class="warning-screenshot" />
</div>

<v-clicks>

- `Environment.TickCount` wraps around after **24 days**
- Prefer `Environment.TickCount64` (wraps around after **292 million years**)
- Not available on .NET Framework :(

</v-clicks>

<style>
.warning-screenshot {
  margin: 1rem 0;
  border-radius: 8px;
  border: 1px solid var(--jb-border);
  max-height: 300px;
  object-fit: contain;
}
</style>

---
clicks: 4
hide: true
---

# Unchecked arithmetic to the rescue

<div :class="['uc-highlight', { vis: $clicks >= 1, hide: $clicks >= 3 }]"></div>

````md magic-move {at:3}
```csharp
var threshold = Environment.TickCount + ThresholdMs;

do
{
    Run();
}
while (HasAnythingToDo() && Environment.TickCount < threshold);
```

```csharp
var timerStart = Environment.TickCount;

do
{
    Run();
}
while (HasAnythingToDo() && unchecked(Environment.TickCount - timerStart) < ThresholdMs);
```
````

<div v-click="2">

- `int.MinValue - int.MaxValue == 1`

</div>

<div v-click="4">

- Can only be used for intervals up to 49 days

</div>

<style>
.uc-highlight {
  position: absolute;
  border: 2px solid #FF318C;
  border-radius: 4px;
  background: rgba(255, 49, 140, 0.08);
  top: 15.5rem;
  left: 16.3rem;
  width: 15rem;
  height: 1.5rem;
  pointer-events: none;
  z-index: 10;
  opacity: 0;
  transition: opacity 0.3s ease;
}
.uc-highlight.vis { opacity: 1; }
.uc-highlight.hide { opacity: 0; }
</style>

---
clicks: 1
hide: true
---

# How fast?

<script setup>
const maxNs = 160
function pct(ns) {
  return (ns / maxNs * 100).toFixed(2)
}
const gridlines = [
  { label: '50 ns',  bottom: pct(50)  },
  { label: '100 ns', bottom: pct(100) },
  { label: '150 ns', bottom: pct(150) },
]
</script>

<div :class="['bench-chart', { vis: $clicks >= 1 }]">
  <div class="chart-body">
    <div class="y-labels">
      <div v-for="g in gridlines" :key="g.label" class="y-label" :style="{ bottom: g.bottom + '%' }">{{ g.label }}</div>
    </div>
    <div class="chart-plot">
      <div v-for="g in gridlines" :key="'grid-' + g.label" class="h-line" :style="{ bottom: g.bottom + '%' }"></div>
      <div class="bar-group" style="--gi:0">
        <div class="bar-wrap">
          <div class="bar netfw-bar" :style="{ height: pct(149.5141) + '%' }"><span class="bar-val">149.5</span></div>
          <div class="bar net10-bar" :style="{ height: pct(52.9723) + '%' }"><span class="bar-val">53.0</span></div>
        </div>
        <div class="x-label">Now</div>
      </div>
      <div class="bar-group" style="--gi:1">
        <div class="bar-wrap">
          <div class="bar netfw-bar" :style="{ height: pct(39.3420) + '%' }"><span class="bar-val">39.3</span></div>
          <div class="bar net10-bar" :style="{ height: pct(20.8705) + '%' }"><span class="bar-val">20.9</span></div>
        </div>
        <div class="x-label">UtcNow</div>
      </div>
      <div class="bar-group" style="--gi:2">
        <div class="bar-wrap">
          <div class="bar netfw-bar" :style="{ height: pct(19.6526) + '%' }"><span class="bar-val">19.7</span></div>
          <div class="bar net10-bar" :style="{ height: pct(16.4515) + '%' }"><span class="bar-val">16.5</span></div>
        </div>
        <div class="x-label">Stopwatch</div>
      </div>
      <div class="bar-group" style="--gi:3">
        <div class="bar-wrap">
          <div class="bar netfw-bar" :style="{ height: pct(0.9783) + '%' }"><span class="bar-val">1.0</span></div>
          <div class="bar net10-bar" :style="{ height: pct(0.8033) + '%' }"><span class="bar-val">0.8</span></div>
        </div>
        <div class="x-label">TickCount</div>
      </div>
    </div>
  </div>
  <div class="legend">
    <div class="legend-item"><span class="dot netfw-dot"></span>.NET Framework 4.8.1</div>
    <div class="legend-item"><span class="dot net10-dot"></span>.NET 10.0</div>
  </div>
</div>

<style>
.bench-chart {
  height: 300px;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-top: 0.5rem;
  opacity: 0;
  transition: opacity 0.3s ease;
}
.bench-chart.vis { opacity: 1; }
.chart-body {
  flex: 1;
  display: flex;
  gap: 0;
  min-height: 0;
}
.y-labels {
  width: 55px;
  flex-shrink: 0;
  position: relative;
}
.y-label {
  position: absolute;
  right: 4px;
  transform: translateY(50%);
  font-size: 0.68rem;
  color: var(--jb-text-soft);
  font-family: 'JetBrains Mono', monospace;
  text-align: right;
  white-space: nowrap;
}
.chart-plot {
  flex: 1;
  position: relative;
  border-left: 1px solid var(--jb-border);
  border-bottom: 1px solid var(--jb-border);
  display: flex;
  align-items: flex-end;
  padding: 0 2rem;
  gap: 1.5rem;
}
.h-line {
  position: absolute;
  left: 0;
  right: 0;
  height: 1px;
  background: var(--jb-border);
  pointer-events: none;
  opacity: 0.5;
}
.bar-group {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
}
.bar-wrap {
  flex: 1;
  width: 100%;
  display: flex;
  align-items: flex-end;
  gap: 3px;
}
.bar {
  flex: 1;
  border-radius: 3px 3px 0 0;
  min-height: 2px;
  position: relative;
  transform: scaleY(0);
  transform-origin: bottom;
  transition: transform 0.6s ease-out;
  transition-delay: calc(var(--gi, 0) * 0.2s);
}
.bench-chart.vis .bar { transform: scaleY(1); }
.bar-val {
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  font-size: 0.55rem;
  white-space: nowrap;
  color: var(--jb-text-soft);
  font-family: 'JetBrains Mono', monospace;
  padding-bottom: 2px;
}
.net10-bar { background: #087CFA; }
.netfw-bar { background: #FC801D; }
.x-label {
  font-size: 0.8rem;
  margin-top: 0.4rem;
  color: var(--jb-text);
  font-weight: 500;
  opacity: 0;
  transition: opacity 0.4s ease;
  transition-delay: calc(var(--gi, 0) * 0.2s);
}
.bench-chart.vis .x-label { opacity: 1; }
.legend {
  display: flex;
  gap: 2rem;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.4s ease 0.7s;
}
.bench-chart.vis .legend { opacity: 1; }
.legend-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.8rem;
}
.dot {
  width: 12px;
  height: 12px;
  border-radius: 2px;
}
.net10-dot { background: #087CFA; }
.netfw-dot { background: #FC801D; }
</style>

---

# Dictionary&lt;K, V&gt; — pick the right comparer

<div class="dict-grid">
  <div v-click="1" class="dict-col">

```csharp
var d = new Dictionary<string, X>(
    StringComparer.InvariantCultureIgnoreCase);
```

<div v-click="2" class="dict-note">

`InvariantCultureIgnoreCase` → **culture-aware** comparisons

German **ß** equals **ss**, Turkish **İ** equals **I**...

</div>
  </div>
  <div v-click="3" class="dict-col">

```csharp
var d = new Dictionary<string, X>(
    StringComparer.OrdinalIgnoreCase);
```

<div v-click="4" class="dict-note">

`OrdinalIgnoreCase` → **byte-level** compare, **much** faster

</div>
  </div>
</div>

<div v-click="5" class="dict-screenshots">
  <img src="/OrdinalIgnoreCase1.png" class="dict-img" />
  <img src="/OrdinalIgnoreCase2.png" class="dict-img" />
</div>

<style>
.dict-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-top: 0.5rem;
}
.dict-col {
  display: flex;
  flex-direction: column;
}
.dict-note {
  font-size: 0.85rem;
  margin-top: 0.3rem;
  padding: 0 0.2rem;
}
.dict-screenshots {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  margin-top: 0.8rem;
}
.dict-img {
  max-height: 215px;
  border-radius: 8px;
  border: 1px solid var(--jb-border);
}
</style>

---
layout: section
hide: true
---

# Deep Dive: The GC

A lot of the remaining UI freezes are caused by the GC

---
hide: true
---
# GC: Crash course

<v-click>

Two types of garbage collections:

</v-click>

<div class="gc-indent">
<v-clicks>

- **Foreground (blocking):** suspends all threads, causes UI freezes
- **Background (BGC):** runs concurrently, can't compact memory

</v-clicks>
</div>

<v-click>

Ephemeral collections (generations 0 and 1) are always foreground, but are expected to be short

</v-click>

<v-click>

Full collections (generation 2) are background, except when compaction is needed

</v-click>

<style>
.gc-indent { padding-left: 2rem; }
</style>

---
hide: true
---

# GC: Watch out for allocations


<v-click>

Fewer allocations ⇒ fewer GCs ⇒ fewer freezes

</v-click>

<v-click>

Not all allocations are equal.

</v-click>

<div class="alloc-grid">
<div v-click class="alloc-section">

**Lifetime:**

<div class="alloc-indent">

- **Short:** die in generation 0 or 1, low impact
- **Long:** survive to generation 2, cause fragmentation and eventually cause a full blocking collection

</div>
</div>
<div v-click class="alloc-section">

**Size:**

<div class="alloc-indent">

- **< 85,000 bytes:** allocated in generation 0
- **≥ 85,000 bytes:** allocated in LOH (Large Object Heap)

</div>
</div>
</div>

<style>
.alloc-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-top: 0.5rem;
}
.alloc-section {
  padding: 0.8rem 1rem;
  background: var(--jb-bg-soft);
  border-radius: 10px;
  border: 1px solid var(--jb-border);
  font-size: 0.9rem;
}
.alloc-indent { padding-left: 1.5rem; }
</style>

---
hide: true
---
# GC: Large Object Heap

<div class="loh-quote">
  <div v-click class="loh-citation">"Allocating in the LOH is bad"</div>
  <div v-click class="loh-author">— Most .NET developers (maybe)</div>
</div>

<v-click>

Why?

</v-click>

<div class="loh-reasons">
  <div v-click class="loh-reason loh-yellow">LOH causes fragmentation</div>
  <div v-click class="loh-reason loh-orange">LOH triggers gen 2 (full) collections</div>
  <div v-click class="loh-reason loh-red">LOH allocations are blocked by the Background GC</div>
</div>

<style>
.loh-quote {
  text-align: center;
  margin: 1.5rem 0 1rem;
}
.loh-citation {
  font-size: 1.5rem;
  font-style: italic;
  color: #444;
}
.loh-author {
  font-size: 0.9rem;
  color: #888;
  margin-top: 0.3rem;
  text-align: right;
  padding-right: 15%;
}
.loh-reasons {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  margin-top: 1rem;
}
.loh-reason {
  padding: 0.7rem 1.5rem;
  border-radius: 10px;
  font-size: 0.95rem;
  font-weight: 600;
}
.loh-yellow { background: #FFF9DB; border-left: 4px solid #E6B800; color: #665200; }
.loh-orange { background: #FFE8D4; border-left: 4px solid #E85D04; color: #6E2C02; }
.loh-red { background: #FFD6D6; border-left: 4px solid #CC0000; color: #8B0000; font-weight: 700; }
</style>

---
hide: true
---
# GC: BGC 💔 LOH

<v-click>

During the BGC, the allocation lock for Large Object Heap is held

</v-click>

<v-click>

**LOH allocations are blocked while the BGC is running**

</v-click>

<div class="bgc-spacer"></div>

<v-click>

Will cause freezes if the UI thread allocates big objects

</v-click>

<div class="bgc-spacer"></div>

<v-click>

Fixed in .NET Core, still a problem in .NET Framework 😭

</v-click>

<v-click>

Use chunked collections to keep large data structures under 85 KB

</v-click>

<style>
.bgc-indent { padding-left: 2rem; }
.bgc-spacer { height: 0.6rem; }
</style>

---
hide: true
---
# GC: Dependent Handles

<v-click>

A GC handle that keeps a *secondary* object alive as long as the *primary* is reachable

</v-click>

<div v-click class="dh-diagram">
  <div class="dh-box dh-primary">Primary</div>
  <div class="dh-arrow">
    <div class="dh-line"></div>
    <div class="dh-label">dependent handle</div>
  </div>
  <div class="dh-box dh-secondary">Secondary</div>
</div>

<v-click>

Behaves exactly as if Primary had a reference to Secondary — **except it doesn't**

</v-click>

<v-click>

Exposed through two APIs:

</v-click>

<div class="dh-indent">
<v-clicks>

- `DependentHandle` only on .NET Core
- `ConditionalWeakTable<TKey, TValue>` Key = primary, Value = secondary

</v-clicks>
</div>

<style>
.dh-diagram {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0;
  margin: 1.5rem 0;
}
.dh-box {
  padding: 0.8rem 2rem;
  border-radius: 10px;
  font-weight: 700;
  font-size: 1.1rem;
  text-align: center;
}
.dh-primary {
  background: #E8F0FE;
  border: 2px solid #087CFA;
  color: #087CFA;
}
.dh-secondary {
  background: #FFF3E0;
  border: 2px solid #E85D04;
  color: #E85D04;
}
.dh-arrow {
  width: 160px;
  position: relative;
  display: flex;
  align-items: center;
  margin-right: 0.3rem;
}
.dh-line {
  width: 100%;
  height: 2px;
  background: #666;
  position: relative;
}
.dh-line::after {
  content: '▶';
  position: absolute;
  right: -6px;
  top: 50%;
  transform: translateY(-50%);
  color: #666;
  font-size: 0.7rem;
  line-height: 1;
}
.dh-label {
  position: absolute;
  bottom: -1.2rem;
  left: 0;
  right: 0;
  text-align: center;
  font-size: 0.7rem;
  color: #888;
  font-style: italic;
}
.dh-indent { padding-left: 2rem; }
</style>

---
clicks: 7
hide: true
---

# GC: GC 💔 Dependent Handles

<div v-click="1" class="dh2-chain">
  <div class="dh2-root">Root</div>
  <div class="dh2-arrow dh2-solid"><div class="dh2-aline"></div></div>
  <div :class="['dh2-node', $clicks >= 2 && 'dh2-alive']">A</div>
  <div class="dh2-arrow dh2-dashed"><div class="dh2-aline"></div><div class="dh2-alabel">dep. handle</div></div>
  <div :class="['dh2-node', $clicks >= 4 && 'dh2-alive']">B</div>
  <div class="dh2-arrow dh2-solid"><div class="dh2-aline"></div></div>
  <div :class="['dh2-node', $clicks >= 5 && 'dh2-alive']">C</div>
  <div class="dh2-arrow dh2-dashed"><div class="dh2-aline"></div><div class="dh2-alabel">dep. handle</div></div>
  <div :class="['dh2-node', $clicks >= 6 && 'dh2-alive']">D</div>
</div>

<div class="dh2-steps">
  <div v-click="2" class="dh2-step">Normal marking: Root → <strong>A</strong> is reachable ✅</div>
  <div v-click="3" class="dh2-step dh2-scan">Scan dependent handles...</div>
  <div v-click="4" class="dh2-step">Pass 1: A is alive → mark <strong>B</strong> ✅</div>
  <div v-click="5" class="dh2-step">Scan newly reachable objects: B → <strong>C</strong> ✅</div>
  <div v-click="6" class="dh2-step">Pass 2: C is alive → mark <strong>D</strong> ✅</div>
</div>

<div v-click="7" class="dh2-punch">

**N** dependent handles → up to **N** extra GC passes — **single-threaded**

</div>

<style>
.dh2-chain {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 1.5rem 0 1.5rem;
  gap: 0;
}
.dh2-root {
  font-weight: 700;
  font-size: 0.9rem;
  color: #666;
  margin-right: 0.3rem;
}
.dh2-node {
  padding: 0.5rem 1.2rem;
  border-radius: 10px;
  font-weight: 700;
  font-size: 1.05rem;
  background: #f0f1f3;
  border: 2px solid #ccc;
  color: #999;
  transition: all 0.4s ease;
}
.dh2-node.dh2-alive {
  background: #dcfce7;
  border-color: #16a34a;
  color: #16a34a;
}
.dh2-arrow {
  width: 80px;
  position: relative;
  display: flex;
  align-items: center;
  margin: 0 0.3rem;
}
.dh2-solid .dh2-aline {
  width: 100%;
  height: 2px;
  background: #666;
  position: relative;
}
.dh2-solid .dh2-aline::after {
  content: '';
  position: absolute;
  right: -2px;
  top: 50%;
  transform: translateY(-50%);
  border-left: 7px solid #666;
  border-top: 5px solid transparent;
  border-bottom: 5px solid transparent;
}
.dh2-dashed .dh2-aline {
  width: 100%;
  height: 2px;
  background: repeating-linear-gradient(to right, #999 0, #999 5px, transparent 5px, transparent 10px);
  position: relative;
}
.dh2-dashed .dh2-aline::after {
  content: '';
  position: absolute;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  border-left: 7px solid #999;
  border-top: 5px solid transparent;
  border-bottom: 5px solid transparent;
}
.dh2-alabel {
  position: absolute;
  bottom: -1.2rem;
  left: 0;
  right: 0;
  text-align: center;
  font-size: 0.65rem;
  color: #aaa;
  font-style: italic;
}
.dh2-steps {
  margin-top: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}
.dh2-step {
  font-size: 0.9rem;
  padding: 0.3rem 0;
}
.dh2-scan {
  color: #888;
  font-style: italic;
}
.dh2-punch {
  margin-top: 1.2rem;
  padding: 0.8rem 1.2rem;
  background: #FFD6D6;
  border-left: 4px solid #CC0000;
  border-radius: 8px;
  font-size: 0.9rem;
  color: #8B0000;
}
</style>

---
hide: true
---
# GC: Dependent Handles in ReSharper

<v-click>

**Solution-Wide Analysis (SWA)** uses CWT to track analysis contexts

</v-click>

<v-click>

Each analysis item → 1 CWT entry → 1 dependent handle

</v-click>

<v-click>

User edits code → new tasks enqueued → handles **accumulate**

</v-click>

<div v-click class="cwt-before-after">
  <div class="cwt-ba-card cwt-before">
    <div class="cwt-ba-title">Before</div>
    <div class="cwt-ba-content">1 CWT entry <strong>per analysis item</strong></div>
    <div class="cwt-ba-result">→ thousands of dependent handles</div>
  </div>
  <div class="cwt-ba-card cwt-after">
    <div class="cwt-ba-title">After</div>
    <div class="cwt-ba-content">1 CWT entry <strong>per chunk</strong> (N items)</div>
    <div class="cwt-ba-result">→ handful of dependent handles</div>
  </div>
</div>

<style>
.cwt-before-after {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-top: 1.2rem;
}
.cwt-ba-card {
  padding: 1rem 1.2rem;
  border-radius: 10px;
  font-size: 0.9rem;
}
.cwt-ba-title {
  font-weight: 700;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 0.5rem;
}
.cwt-before {
  background: #FFD6D6;
  border: 1px solid #CC0000;
}
.cwt-before .cwt-ba-title { color: #CC0000; }
.cwt-before .cwt-ba-result { color: #8B0000; font-weight: 600; }
.cwt-after {
  background: #dcfce7;
  border: 1px solid #16a34a;
}
.cwt-after .cwt-ba-title { color: #16a34a; }
.cwt-after .cwt-ba-result { color: #166534; font-weight: 600; }
</style>

---
hide: true
---
# Visual Studio Has the Same Problem

<div v-click class="vs-screenshot">
  <img src="/dependentHandles1.png" />
</div>

<style>
.vs-screenshot {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}
.vs-screenshot img {
  max-height: 420px;
  border-radius: 8px;
  border: 1px solid var(--jb-border);
}
</style>

---
clicks: 8
---

# Metadata Strings

<div :class="['ms-intro', { visible: $clicks >= 1 }]">

We read **all** metadata from referenced assemblies

</div>

<div :class="['ms-plain', { visible: $clicks >= 2, hidden: $clicks >= 3 }]">
  <code>System.String.get_Length → System.Int32</code>
</div>

<div class="ms-rows">
  <div :class="['ms-row', { visible: $clicks >= 3 }]">
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-system': $clicks >= 5 }">System</div>
    <div class="ms-dot">.</div>
    <div class="ms-box ms-neutral">String</div>
    <div class="ms-dot">.</div>
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-method': $clicks >= 6 }">get_Length</div>
    <div class="ms-arrow">→</div>
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-system': $clicks >= 5 }">System</div>
    <div class="ms-dot">.</div>
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-int': $clicks >= 7 }">Int32</div>
  </div>

  <div :class="['ms-row', { visible: $clicks >= 4 }]">
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-system': $clicks >= 5 }">System</div>
    <div class="ms-dot">.</div>
    <div class="ms-box ms-neutral">Array</div>
    <div class="ms-dot">.</div>
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-method': $clicks >= 6 }">get_Length</div>
    <div class="ms-arrow">→</div>
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-system': $clicks >= 5 }">System</div>
    <div class="ms-dot">.</div>
    <div class="ms-box ms-neutral" :class="{ 'ms-hl-int': $clicks >= 7 }">Int32</div>
  </div>
</div>

<div :class="['ms-dedup', { visible: $clicks >= 8 }]">
  <div class="ms-dedup-label">After deduplication:</div>
  <div class="ms-dedup-row">
    <div class="ms-box ms-hl-system">System</div>
    <div class="ms-box ms-neutral">String</div>
    <div class="ms-box ms-neutral">Array</div>
    <div class="ms-box ms-hl-method">get_Length</div>
    <div class="ms-box ms-hl-int">Int32</div>
  </div>
  <div class="ms-result">~50% fewer string allocations</div>
</div>

<style>
.ms-intro {
  opacity: 0;
  transition: opacity 0.4s ease;
}
.ms-intro.visible { opacity: 1; }
.ms-plain {
  margin-top: 1.5rem;
  opacity: 0;
  transition: opacity 0.4s ease;
}
.ms-plain.visible { opacity: 1; }
.ms-plain.hidden { opacity: 0; position: absolute; }
.ms-plain code {
  font-family: 'Cascadia Mono', 'JetBrains Mono', monospace;
  font-size: 1.1rem;
  background: none;
  color: #333;
}
.ms-rows { margin-top: 1.5rem; }
.ms-row {
  display: flex;
  align-items: center;
  gap: 0.15rem;
  margin-bottom: 0.8rem;
  opacity: 0;
  transform: translateY(8px);
  transition: all 0.4s ease;
}
.ms-row.visible { opacity: 1; transform: translateY(0); }
.ms-box {
  padding: 0.35rem 0.7rem;
  border-radius: 5px;
  font-family: 'Cascadia Mono', 'JetBrains Mono', monospace;
  font-size: 0.85rem;
  font-weight: 600;
  transition: all 0.5s ease;
}
.ms-neutral { background: #e8eaed; color: #333; border: 1px solid #ccc; }
.ms-hl-system { background: #dbeafe; color: #1d4ed8; border-color: #60a5fa; }
.ms-hl-method { background: #dcfce7; color: #166534; border-color: #4ade80; }
.ms-hl-int { background: #f3e8ff; color: #6b21a8; border-color: #c084fc; }
.ms-dot { font-size: 1rem; color: #999; margin: 0 0.05rem; font-weight: 700; }
.ms-arrow { font-size: 1.1rem; color: #666; margin: 0 0.5rem; }
.ms-dedup {
  margin-top: 1.2rem;
  opacity: 0;
  transform: translateY(8px);
  transition: all 0.4s ease;
}
.ms-dedup.visible { opacity: 1; transform: translateY(0); }
.ms-dedup-label {
  font-size: 0.85rem;
  color: #666;
  margin-bottom: 0.5rem;
  font-weight: 600;
}
.ms-dedup-row {
  display: flex;
  gap: 0.4rem;
}
.ms-result {
  margin-top: 0.8rem;
  font-size: 1.05rem;
  font-weight: 700;
  color: #166534;
}
</style>

---

# The Ominous Comment

<div v-click class="intern-comment">

```
// kskrygan: do not cache/intern here
// 1 reason: memory traffic in ChunkSparseArray
//           (needed to avoid LOH allocations)
// 2 reason: contentions on the following lock statement
//           (we load assemblies simultaneously in multiple threads)
// 3 reason: memory needed for myStringsCache
```

</div>

<v-click>

Comment is unambiguous: **DO NOT** dedup (intern)

</v-click>

<v-click>

Somebody tried, and cared enough to leave a comment

</v-click>

<v-click>

Written in **November 2014** — over 11 years ago. Maybe things have changed since?

</v-click>

<style>
.intern-comment pre {
  background: #1e1e1e;
  border-left: 4px solid #e8a32e;
  padding: 1rem 1.2rem;
  font-size: 0.85rem;
  line-height: 1.6;
  border-radius: 6px;
}
</style>

---

# A Week of Careful Testing

<v-clicks>

- Tested various deduping / interning solutions
- Dictionary with lock, ConcurrentDictionary, ImmutableDictionary, ...
- Many different scenarios, various solution sizes

</v-clicks>

<div v-click class="intern-results">
  <div class="intern-card intern-card-before">
    <div class="intern-card-title">Before</div>
    <div class="intern-metric">
      <span class="intern-label">Heap allocations</span>
      <span class="intern-value">4,794 MB</span>
    </div>
    <div class="intern-metric">
      <span class="intern-label">CPU time</span>
      <span class="intern-value">39,481 ms</span>
    </div>
  </div>
  <div class="intern-card intern-card-after">
    <div class="intern-card-title">After</div>
    <div class="intern-metric">
      <span class="intern-label">Heap allocations</span>
      <span class="intern-value">781 MB <span class="intern-pct">(− 84%)</span></span>
    </div>
    <div class="intern-metric">
      <span class="intern-label">CPU time</span>
      <span class="intern-value">16,959 ms <span class="intern-pct">(− 57%)</span></span>
    </div>
  </div>
</div>

<style>
.intern-results {
  margin-top: 2rem;
  display: flex;
  gap: 2rem;
  justify-content: center;
}
.intern-card {
  flex: 1;
  max-width: 380px;
  padding: 1.2rem 1.5rem;
  border-radius: 10px;
  border: 1px solid var(--jb-border);
}
.intern-card-before {
  background: rgba(239, 68, 68, 0.08);
  border-color: rgba(239, 68, 68, 0.3);
}
.intern-card-after {
  background: rgba(74, 222, 128, 0.08);
  border-color: rgba(74, 222, 128, 0.3);
}
.intern-card-title {
  font-weight: 700;
  font-size: 1.1rem;
  margin-bottom: 0.8rem;
}
.intern-card-before .intern-card-title { color: #f87171; }
.intern-card-after .intern-card-title { color: #22c55e; }
.intern-metric {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding: 0.3rem 0;
}
.intern-label {
  color: #aaa;
  font-size: 0.95rem;
}
.intern-value {
  font-family: 'Cascadia Mono', monospace;
  font-weight: 700;
  font-size: 1.15rem;
}
.intern-pct {
  color: #22c55e;
  font-size: 0.95rem;
}
</style>

---
clicks: 5
---

# But Who Wrote That Comment?

<v-click>

`git blame` → **Kirill Skrygan**

</v-click>

<v-click>

Let me look him up in the company directory...

</v-click>

<div v-click class="ceo-reveal">
  <img src="/ceo.png" />
</div>

<div :class="['diff-overlay', { visible: $clicks >= 5 }]">
  <img src="/diff.png" />
</div>

<div :class="['ceo-deadpan', { visible: $clicks >= 4 }]">
Yeah, maybe I'm not going to ask.
</div>

<style>
.ceo-reveal {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}
.ceo-reveal img {
  max-height: 280px;
  border-radius: 12px;
  border: 1px solid var(--jb-border);
  box-shadow: 0 4px 20px rgba(0,0,0,0.15);
}
.diff-overlay {
  position: absolute;
  inset: 0;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.5s ease, transform 0.5s ease;
  pointer-events: none;
}
.diff-overlay.visible {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
.diff-overlay img {
  max-width: 90%;
  max-height: 85%;
}
.ceo-deadpan {
  text-align: center;
  margin-top: 1.2rem;
  font-size: 1.3rem;
  font-style: italic;
  color: #666;
  opacity: 0;
  transition: opacity 0.6s ease 0.6s;
}
.ceo-deadpan.visible {
  opacity: 1;
}
</style>

---

# BigInteger: The Licensing Code

<v-clicks>

- Licensing used an **old open-source BigInteger** (pre-`System.Numerics`)
- It was a **reference type** with an underlying **array**

</v-clicks>

<div v-click>

```csharp
public class BigInteger
{
    private const int maxLength = 70;

    // Every operation creates a new instance + new array
    private uint[] data = new uint[maxLength];
    
    public static BigInteger operator +(BigInteger a, BigInteger b)
    {
        var result = new BigInteger();  // allocation!
        // ...
        return result;
    }
}
```

</div>



<style>
.bigint-note {
  text-align: center; font-size: 0.9rem; color: #FF318C;
  margin-top: 1rem; font-weight: 500;
}
</style>

---

# BigInteger: The Fix

Rewrote as a **large struct** using `unsafe fixed`:

```csharp
public unsafe struct BigInteger
{
    private fixed uint data[maxLength];  // embedded in the struct, no heap allocation
    
    public static BigInteger operator +(in BigInteger a, in BigInteger b)
    {
        BigInteger result = default;  // stack-allocated, no GC pressure
        // ...
        return result;
    }
}
```

<v-clicks>

- `InlineArray` would be cleaner but **doesn't work on .NET Framework**
- Careful use of **`ref`** / **`in`** to avoid copying the large struct

</v-clicks>

---
clicks: 5
---

# BigInteger: beware when using fixed

<div class="zeroing-grid">
  <div v-click="1" class="zeroing-col">

```csharp
private struct StructWithFixedBuffer
{

    public fixed long buffer[1];
}

for (int i = 0; i < 10; i++)
{
    var a = new StructWithFixedBuffer();
    Console.Write($"{a.buffer[0]} ");
    a.buffer[0]++;
}
```

<div v-click="2" class="zeroing-output">

```
0 0 0 0 0 0 0 0 0 0
```

</div>
  </div>
  <div v-click="3" class="zeroing-col">

```csharp
private struct StructWithFixedBuffer
{
    public StructWithFixedBuffer() { }
    public fixed long buffer[1];
}

for (int i = 0; i < 10; i++)
{
    var a = new StructWithFixedBuffer();
    Console.Write($"{a.buffer[0]} ");
    a.buffer[0]++;
}
```

<div v-click="4" class="zeroing-output zeroing-bad">

```
0 1 2 3 4 5 6 7 8 9
```

</div>
  </div>
</div>

<div :class="['egor-overlay', { visible: $clicks >= 5 }]">
  <img src="/egor.png" class="egor-img" />
</div>

<style>
.zeroing-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;
}
.zeroing-col {
  min-width: 0;
}
.zeroing-output {
  text-align: center; margin-top: 0.5rem; font-size: 1.1rem;
  padding: 0.4rem 0.8rem; color: #4EC9B0;
}
.zeroing-output code {
  background: none !important; border: none !important;
  color: inherit !important; padding: 0 !important;
}
.zeroing-bad {
  color: #FF318C;
}
.zeroing-note {
  text-align: center; margin-top: 1rem; font-size: 0.95rem;
  color: #FF318C; font-weight: 500;
}
.egor-overlay {
  position: absolute; inset: 0; z-index: 10;
  display: flex; align-items: center; justify-content: center;
  opacity: 0; transform: translateY(40px);
  transition: opacity 0.5s ease, transform 0.5s ease;
  pointer-events: none;
}
.egor-overlay.visible {
  opacity: 1; transform: translateY(0);
  pointer-events: auto;
}
.egor-img {
  max-width: 90%; max-height: 85%;
}
</style>

---

# BigInteger: The Fix

```csharp
private fixed uint data[maxLength];

public BigInteger()
{
    ClearData();
    // ...
}

private void ClearData()
{
    fixed (uint* d = data)
    {
        new Span<uint>(d, maxLength).Clear();
    }
}
```

---

# BigInteger: Results

<div class="bigint-compare">
  <div v-click>
    <img src="/bigInteger1.jpg" class="bigint-img" />
    <div class="bigint-caption">Before</div>
  </div>
  <div v-click class="bigint-arrow">→</div>
  <div v-click>
    <img src="/bigInteger2.jpg" class="bigint-img" />
    <div class="bigint-caption">After</div>
  </div>
</div>

<div class="bigint-stats">
  <div v-click class="stat">
    <div class="stat-val">147 MB → 6.3 MB</div>
    <div class="stat-label">allocations</div>
  </div>
  <div v-click class="stat">
    <div class="stat-val">-95%</div>
    <div class="stat-label">reduction</div>
  </div>
</div>

<style>
.bigint-compare {
  display: flex; align-items: center; justify-content: center;
  gap: 1rem; margin: 1rem 0;
}
.bigint-img { max-height: 590px; border-radius: 8px; border: 1px solid var(--jb-border); }
.bigint-caption { text-align: center; font-size: 0.8rem; opacity: 0.6; margin-top: 0.3rem; }
.bigint-arrow { font-size: 2rem; opacity: 0.4; }
.bigint-stats {
  display: flex; gap: 2rem; justify-content: center; margin: 1rem 0;
}
.stat { text-align: center; }
.stat-val { font-size: 1.3rem; font-weight: 700; color: #087CFA; }
.stat-label { font-size: 0.8rem; opacity: 0.6; }
.bigint-note {
  text-align: center; font-size: 0.85rem; opacity: 0.8; margin-top: 0.5rem;
}
</style>

---
clicks: 6
---

# Beware of Incorrect Assumptions

<v-click>

`GetModuleReference` — Finds a specific module in the project references

</v-click>

<v-click>

Uses a **List** as the underlying storage

</v-click>

<div v-click class="ref-question">
  <div>How many references can a typical project have?</div>
</div>

<div v-click class="ref-question">
  <code>dotnet new webapi</code>
</div>

<div v-click class="ref-reveal">
  <span class="ref-counter">{{ displayCount }}</span> references
  <div :class="['ref-sub', { visible: showSub }]">before even adding any external library</div>
</div>

<div v-click class="ref-before-after">
  <div class="ref-ba-card ref-before">
    <div class="ref-ba-title">Before (List)</div>
    <div class="ref-ba-content">CPU time: <strong>39,136 ms</strong></div>
  </div>
  <div class="ref-ba-card ref-after">
    <div class="ref-ba-title">After (Dictionary)</div>
    <div class="ref-ba-content">CPU time: <strong>1,736 ms</strong> <span class="ref-ba-result">(-95%)</span></div>
  </div>
</div>

<style>
.ref-question {
  margin-top: 1.5rem; font-size: 1.3rem; text-align: center;
}
.ref-reveal {
  text-align: center; margin-top: 1rem; font-size: 1.2rem;
}
.ref-reveal code {
  background: none !important; border: none !important;
  color: #087CFA !important; font-size: 1.1rem;
}
.ref-counter {
  font-size: 2.5rem; font-weight: 900; color: #FF318C;
  font-variant-numeric: tabular-nums;
}
.ref-sub {
  font-size: 0.85rem; opacity: 0; margin-top: 0.2rem; color: #333;
  transition: opacity 0.5s ease;
}
.ref-sub.visible {
  opacity: 0.6;
}
.ref-before-after {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 1.5rem; margin-top: 1.5rem;
}
.ref-ba-card {
  padding: 1rem 1.2rem; border-radius: 10px; font-size: 0.95rem;
}
.ref-ba-title {
  font-weight: 700; font-size: 0.8rem; text-transform: uppercase;
  letter-spacing: 0.05em; margin-bottom: 0.5rem;
}
.ref-ba-content { font-size: 1.1rem; }
.ref-ba-result { margin-top: 0.3rem; font-weight: 600; }
.ref-before {
  background: #FFD6D6; border: 1px solid #CC0000;
}
.ref-before .ref-ba-title { color: #CC0000; }
.ref-before .ref-ba-result { color: #8B0000; }
.ref-after {
  background: #dcfce7; border: 1px solid #16a34a;
}
.ref-after .ref-ba-title { color: #16a34a; }
.ref-after .ref-ba-result { color: #166534; }
</style>

<script setup>
import { ref, watch } from 'vue'
import { useSlideContext } from '@slidev/client'

const ctx = useSlideContext()
const displayCount = ref(0)
const showSub = ref(false)
let animFrame = 0
let animated = false

watch(() => ctx.$clicks.value, (val) => {
  if (val >= 5 && !animated) animateCounter()
})

function animateCounter() {
  animated = true
  cancelAnimationFrame(animFrame)
  const duration = 8000
  const target = 309
  const start = performance.now()
  function tick(now) {
    const t = Math.min((now - start) / duration, 1)
    const eased = t * t
    displayCount.value = Math.round(eased * target)
    if (t < 1) {
      animFrame = requestAnimationFrame(tick)
    } else {
      showSub.value = true
    }
  }
  animFrame = requestAnimationFrame(tick)
}
</script>

---
hide: true
---

# Shutdowns got some love too

<div v-click class="on2-problem">
  Problem: shutdown takes a long time on large solutions (up to <strong>45 seconds</strong>)
</div>

<v-click>

The settings store keeps projects in a **watchable collection**

</v-click>

<v-click>

A subscriber listens to changes and **rebuilds the entire collection as a snapshot** on every event

</v-click>

<v-click>

During shutdown, projects are removed **one by one** — each removal triggers a full rebuild

</v-click>

<div v-click class="on2-fix">
  Fix: handle add/remove <strong>individually</strong> → shutdown down to <strong>~10 seconds</strong>
</div>

<div v-click class="dawson-quote">
  <div class="dawson-text">"O(n²) is the sweet spot of badly scaling algorithms: fast enough to make it into production, but slow enough to make things fall down once it gets there."</div>
  <div class="dawson-author">— Bruce Dawson</div>
</div>

<style>
.dawson-quote {
  margin: 1rem 0 1.5rem; padding: 1rem 1.5rem;
  border-left: 4px solid #087CFA; background: var(--jb-bg-soft);
  border-radius: 0 8px 8px 0;
}
.dawson-text {
  font-size: 0.95rem; font-style: italic; line-height: 1.5;
}
.dawson-author {
  margin-top: 0.5rem; font-size: 0.85rem; opacity: 0.6; font-style: normal;
}
.on2-problem {
  text-align: center; margin-top: 1rem; margin-bottom: 1rem; font-size: 1.05rem;
  padding: 0.6rem 1.2rem; background: #FFD6D6; border-radius: 8px;
  color: #CC0000;
}
.on2-fix {
  text-align: center; margin-top: 1rem; font-size: 1.05rem;
  padding: 0.6rem 1.2rem; background: #dcfce7; border-radius: 8px;
  color: #166534;
}
</style>

---
layout: section
hide: true
---

# The Results

---
layout: image
image: /results.png
clicks: 5
---

<div class="rs-chart-upper">
  <div :class="['rs-group', { vis: $clicks >= 1 }]">
    <div class="rs-name">Console</div>
    <div :class="['rs-row', { vis: $clicks >= 2 }]"><div class="rs-bar rs-old" style="--w:8.8"></div><span class="rs-v">9.9s</span></div>
    <div :class="['rs-row', { vis: $clicks >= 3 }]"><div class="rs-bar rs-new" style="--w:1.3"></div><span class="rs-v">1.5s</span><span class="rs-p">-85%</span></div>
    <div :class="['rs-row', { vis: $clicks >= 5 }]"><div class="rs-bar rs-oop" style="--w:0.8"></div><span class="rs-v">0.9s</span><span class="rs-p rs-pg">-91%</span></div>
  </div>
  <div :class="['rs-group', { vis: $clicks >= 1 }]">
    <div class="rs-name">Orchard</div>
    <div :class="['rs-row', { vis: $clicks >= 2 }]"><div class="rs-bar rs-old" style="--w:16.1"></div><span class="rs-v">18s</span></div>
    <div :class="['rs-row', { vis: $clicks >= 3 }]"><div class="rs-bar rs-new" style="--w:4.7"></div><span class="rs-v">5.3s</span><span class="rs-p">-71%</span></div>
    <div :class="['rs-row', { vis: $clicks >= 5 }]"><div class="rs-bar rs-oop" style="--w:2.4"></div><span class="rs-v">2.7s</span><span class="rs-p rs-pg">-85%</span></div>
  </div>
</div>

<div class="rs-chart-lower">
  <div :class="['rs-group', { vis: $clicks >= 1 }]">
    <div class="rs-name">Datadog</div>
    <div :class="['rs-row', 'rs-row-wide', { vis: $clicks >= 2 }]"><div class="rs-bar rs-old" style="--w:100"></div><span class="rs-v">112s</span></div>
    <div :class="['rs-row', 'rs-row-wide', { vis: $clicks >= 3 }]"><div class="rs-bar rs-new" style="--w:52.7"></div><span class="rs-v">59s</span><span class="rs-p">-47%</span></div>
    <div :class="['rs-row', 'rs-row-wide', { vis: $clicks >= 5 }]"><div class="rs-bar rs-oop" style="--w:29.5"></div><span class="rs-v">33s</span><span class="rs-p rs-pg">-71%</span></div>
  </div>
</div>

<div :class="['rs-legend', { vis: $clicks >= 2 }]">
  <span class="rs-lg"><span class="rs-dot rs-old"></span>R# 2025.1</span>
  <span :class="['rs-lg', 'rs-fade', { vis: $clicks >= 3 }]"><span class="rs-dot rs-new"></span>R# 2026.1</span>
  <span :class="['rs-lg', 'rs-fade', { vis: $clicks >= 5 }]"><span class="rs-dot rs-oop"></span>R# 2026.1 OOP</span>
</div>

<div :class="['rs-loads', { vis: $clicks >= 4 }]">
  <div class="rs-load-row">
    <div class="rs-load-name">Console</div>
    <div class="rs-load-nums"><span class="rs-num-old">21.6s</span> → <span class="rs-num-new">20.3s</span><span :class="['rs-load-tail', { vis: $clicks >= 5 }]"> → <span class="rs-num-oop">11.1s</span></span></div>
  </div>
  <div class="rs-load-row">
    <div class="rs-load-name">Orchard</div>
    <div class="rs-load-nums"><span class="rs-num-old">47.5s</span> → <span class="rs-num-new">47.2s</span><span :class="['rs-load-tail', { vis: $clicks >= 5 }]"> → <span class="rs-num-oop">39s</span></span></div>
  </div>
  <div class="rs-load-row">
    <div class="rs-load-name">Datadog</div>
    <div class="rs-load-nums"><span class="rs-num-old">167s</span> → <span class="rs-num-new">125s</span><span :class="['rs-load-tail', { vis: $clicks >= 5 }]"> → <span class="rs-num-oop">102s</span></span></div>
  </div>
</div>

<style>
.rs-chart-upper, .rs-chart-lower, .rs-legend, .rs-loads {
  font-feature-settings: 'liga' 0, 'calt' 0, 'dlig' 0, 'clig' 0, 'swsh' 0;
  font-variant-ligatures: none;
}
.rs-chart-upper {
  position: absolute;
  left: 26%; right: 34%;
  top: 30%; bottom: 30%;
  display: flex; flex-direction: column;
  gap: 0.4rem;
  color: #2d1b00;
  font-family: 'Alegreya', serif;
}
.rs-chart-lower {
  position: absolute;
  left: 26%; right: 7%;
  top: 71%; bottom: 13%;
  display: flex; flex-direction: column;
  color: #2d1b00;
  font-family: 'Alegreya', serif;
}
.rs-group {
  display: flex; flex-direction: column;
  gap: 1px;
  opacity: 0;
  transition: opacity 0.35s ease;
}
.rs-group.vis { opacity: 1; }
.rs-name {
  font-weight: 700; font-size: 1.1rem;
  color: #3a1d04;
  margin-bottom: 0.1rem;
  font-family: 'Alegreya', serif;
  letter-spacing: 0.01em;
}
.rs-row {
  display: flex; align-items: center; gap: 0.5rem;
  opacity: 0;
  transition: opacity 0.4s ease;
  min-height: 20px;
}
.rs-row.vis { opacity: 1; }
.rs-bar {
  height: 20px; border-radius: 2px; min-width: 6px;
  width: calc(var(--w) * 1.2%);
  max-width: 78%;
  border: 1px solid rgba(40, 20, 5, 0.7);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.2),
    inset 0 -1px 0 rgba(0, 0, 0, 0.18),
    0 2px 3px rgba(60, 30, 0, 0.35);
  flex-shrink: 0;
}
.rs-row-wide .rs-bar {
  width: calc(var(--w) * 0.72%);
  max-width: 80%;
}
.rs-old {
  background: linear-gradient(180deg, #b22a2a 0%, #8b1f17 50%, #5d1108 100%);
}
.rs-new {
  background: linear-gradient(180deg, #2c5fa8 0%, #1e3a8a 55%, #14245c 100%);
}
.rs-oop {
  background: linear-gradient(180deg, #2d8b4e 0%, #166534 55%, #0c3a1d 100%);
}
.rs-v {
  font-size: 0.95rem; font-weight: 500; color: #2d1b00;
  white-space: nowrap;
  font-family: 'Alegreya', serif;
}
.rs-p {
  font-size: 1.05rem; font-weight: 700;
  color: #1e3a8a;
  line-height: 1;
  font-family: 'Alegreya', serif;
  flex-shrink: 0;
  margin-left: 0.4rem;
}
.rs-pg { color: #14532d; }

.rs-legend {
  position: absolute;
  left: 26%; right: 34%;
  bottom: 4.5%;
  display: flex; gap: 1.3rem;
  font-size: 0.9rem; font-weight: 500;
  color: #2d1b00;
  opacity: 0; transition: opacity 0.4s;
  font-family: 'Alegreya', serif;
}
.rs-legend.vis { opacity: 1; }
.rs-lg { display: flex; align-items: center; gap: 0.4rem; }
.rs-fade { opacity: 0; transition: opacity 0.4s; }
.rs-fade.vis { opacity: 1; }
.rs-dot {
  width: 12px; height: 12px; border-radius: 2px; display: inline-block;
  border: 1px solid rgba(40, 20, 5, 0.7);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2);
}
.rs-dot.rs-old { background: linear-gradient(180deg, #b22a2a, #5d1108); }
.rs-dot.rs-new { background: linear-gradient(180deg, #2c5fa8, #14245c); }
.rs-dot.rs-oop { background: linear-gradient(180deg, #2d8b4e, #0c3a1d); }

.rs-loads {
  position: absolute;
  left: 73%; right: 6%;
  top: 28%; bottom: 26%;
  display: flex; flex-direction: column;
  gap: 0.6rem;
  color: #2d1b00;
  opacity: 0; transition: opacity 0.4s;
  font-family: 'Alegreya', serif;
}
.rs-loads.vis { opacity: 1; }
.rs-load-row {
  display: flex; flex-direction: column;
  gap: 0.05rem;
}
.rs-load-name {
  font-weight: 700; font-size: 1.1rem;
  color: #3a1d04;
  letter-spacing: 0.01em;
}
.rs-load-nums {
  font-size: 1.15rem; font-weight: 500;
  color: #2d1b00;
}
.rs-load-tail {
  opacity: 0; transition: opacity 0.4s;
}
.rs-load-tail.vis { opacity: 1; }
.rs-num-old { color: #8b1f17; font-weight: 500; }
.rs-num-new { color: #1e3a8a; font-weight: 500; }
.rs-num-oop { color: #166534; font-weight: 700; }
</style>

---
clicks: 1
---

<div v-if="$clicks >= 1" class="sxs-fullscreen">
  <video src="/resharper_sxs.mp4" autoplay class="sxs-video" />
</div>

<style>
.sxs-fullscreen {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: black;
  z-index: 9999;
}
.sxs-video {
  max-height: 100%;
  max-width: 100%;
}
</style>

---
layout: image
image: /closing.png
---

<div class="closing-social">
  <div class="closing-social-item"><img src="/twitter.png" class="closing-social-icon" /> @kookiz</div>
  <div class="closing-social-item"><img src="/bluesky.png" class="closing-social-icon" /> @kevingosse.net</div>
  <div class="closing-social-item"><img src="/github.svg" class="closing-social-icon" style="filter: brightness(2);" />kevingosse/the-road-to-a-faster-resharper</div>
  <div class="closing-social-item"><img src="/github.svg" class="closing-social-icon" style="filter: brightness(2);" />kevingosse/silhouette</div>
  <div class="closing-social-item"><img src="/blog.svg" class="closing-social-icon" style="filter: brightness(2);" />minidump.net</div>
</div>

<style>
.closing-social {
  position: absolute;
  bottom: 4%;
  left: 3%;
  z-index: 3;
  display: flex;
  gap: 0.4rem;
}
.closing-social-item {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  color: #e8dcc8;
  font-family: 'Alegreya', serif;
  font-size: 0.9rem;
  background: rgba(0,0,0,0.55);
  padding: 0.25rem 0.6rem;
  border-radius: 4px;
}
.closing-social-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
  filter: brightness(1.5);
}
</style>

