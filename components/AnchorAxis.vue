<script setup lang="ts">
// Conceptual score axis: baseline b, expert e, agent score mu, headroom h = e - b.
const X0 = 40
const X1 = 780
const Y = 112
const b = 0.2
const e = 0.78
const mu = 0.55
const x = (v: number) => X0 + v * (X1 - X0)
</script>

<template>
  <svg viewBox="0 0 820 200" class="axis" role="img" aria-label="Score axis with baseline, expert and agent score">
    <!-- headroom brace -->
    <path :d="`M ${x(b)} 62 q 0 -10 10 -10 L ${(x(b) + x(e)) / 2 - 10} 52 q 10 0 10 -10 q 0 10 10 10 L ${x(e) - 10} 52 q 10 0 10 10`"
          class="brace" />
    <text :x="(x(b) + x(e)) / 2" y="24" class="h">headroom  h = e − b</text>
    <text :x="(x(b) + x(e)) / 2" y="40" class="hs">the part crossed by scientific insight, not by engineering</text>

    <rect :x="x(b)" :y="Y - 9" :width="x(e) - x(b)" height="18" rx="3" class="band" />
    <line :x1="X0" :x2="X1" :y1="Y" :y2="Y" class="ax" />
    <line :x1="X0" :x2="X0" :y1="Y - 7" :y2="Y + 7" class="ax" />
    <line :x1="X1" :x2="X1" :y1="Y - 7" :y2="Y + 7" class="ax" />
    <text :x="X0" :y="Y + 28" class="end">0</text>
    <text :x="X1" :y="Y + 28" class="end">1</text>

    <line :x1="x(b)" :x2="x(b)" :y1="Y - 14" :y2="Y + 14" class="base" />
    <text :x="x(b)" :y="Y + 32" class="lab">b · baseline</text>
    <text :x="x(b)" :y="Y + 50" class="sub">no domain knowledge</text>

    <line :x1="x(e)" :x2="x(e)" :y1="Y - 16" :y2="Y + 16" class="exp" />
    <text :x="x(e)" :y="Y + 32" class="lab">e · expert</text>
    <text :x="x(e)" :y="Y + 50" class="sub">reproduced published method</text>

    <circle :cx="x(mu)" :cy="Y" r="9" class="agent" />
    <text :x="x(mu)" :y="Y + 32" class="lab">μ · agent</text>
    <text :x="x(mu)" :y="Y + 50" class="sub">field's figure of merit</text>
  </svg>
</template>

<style scoped>
.axis { width: 100%; height: auto; }
text { fill: #0b0b0b; text-anchor: middle; }
.h { font-size: 18px; font-weight: 700; fill: #24414c; }
.hs { font-size: 14px; fill: #52514e; }
.lab { font-size: 16px; font-weight: 700; }
.sub { font-size: 13px; fill: #52514e; }
.end { font-size: 14px; fill: #898781; }
.ax { stroke: #898781; stroke-width: 1.5; }
.band { fill: #d5e3e8; }
.base { stroke: #898781; stroke-width: 2.5; stroke-dasharray: 4 3; }
.exp { stroke: #24414c; stroke-width: 3.5; }
.agent { fill: #2a78d6; stroke: #fff; stroke-width: 2.5; }
.brace { fill: none; stroke: #24414c; stroke-width: 1.8; }
</style>
