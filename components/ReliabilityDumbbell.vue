<script setup lang="ts">
// HEP domain: mean over 3 runs vs best of the same 3 runs, per model.
// Source: draft paper, Tables "Per-Model Results" (mean) and "maximum raw task scores".
const rows = [
  { m: 'Gemini 3.7 Flash', mean: 0.37, best: 0.51 },
  { m: 'Gemini 3.1 Pro', mean: 0.57, best: 0.68 },
  { m: 'Claude Opus 5', mean: 0.55, best: 0.70 },
  { m: 'DeepSeek V4 Pro', mean: 0.60, best: 0.71 },
  { m: 'Qwen3.8 2.4T', mean: 0.53, best: 0.64 },
  { m: 'GLM 5.2', mean: 0.46, best: 0.66 },
  { m: 'MiMo V2.5', mean: 0.23, best: 0.41 },
  { m: 'MiniMax M3', mean: 0.32, best: 0.63 },
]
// mean of the four HEP expert anchors: (0.912 + 0.687 + 0.884 + 0.221) / 4
const expert = 0.676

const W = 460
const X0 = 132
const X1 = 446
const TOP = 34
const ROW = 27
const lo = 0.2
const hi = 0.8
const x = (v: number) => X0 + ((v - lo) / (hi - lo)) * (X1 - X0)
const H = TOP + rows.length * ROW + 24
const ticks = [0.2, 0.4, 0.6, 0.8]
</script>

<template>
  <svg :viewBox="`0 0 ${W} ${H}`" class="db" role="img"
       aria-label="HEP mean versus best-of-three score per model">
    <g transform="translate(0,12)" class="legend">
      <circle cx="6" cy="0" r="5" class="mean" />
      <text x="16" y="4">mean of 3 runs</text>
      <circle cx="132" cy="0" r="5" class="best" />
      <text x="142" y="4">best of 3 runs</text>
      <line x1="262" x2="262" y1="-7" y2="7" class="exp" />
      <text x="270" y="4">avg. expert anchor</text>
    </g>
    <g v-for="t in ticks" :key="t">
      <line :x1="x(t)" :x2="x(t)" :y1="TOP - 6" :y2="H - 20" class="grid" />
      <text :x="x(t)" :y="H - 5" class="tick">{{ t.toFixed(1) }}</text>
    </g>
    <line :x1="x(expert)" :x2="x(expert)" :y1="TOP - 6" :y2="H - 20" class="exp" />
    <g v-for="(r, i) in rows" :key="r.m">
      <text :x="X0 - 10" :y="TOP + i * ROW + ROW / 2 + 5" class="name">{{ r.m }}</text>
      <line :x1="x(r.mean)" :x2="x(r.best)" :y1="TOP + i * ROW + ROW / 2" :y2="TOP + i * ROW + ROW / 2" class="link" />
      <circle :cx="x(r.mean)" :cy="TOP + i * ROW + ROW / 2" r="6" class="mean">
        <title>{{ r.m }}: mean {{ r.mean.toFixed(2) }}</title>
      </circle>
      <circle :cx="x(r.best)" :cy="TOP + i * ROW + ROW / 2" r="6" class="best">
        <title>{{ r.m }}: best {{ r.best.toFixed(2) }}</title>
      </circle>
    </g>
  </svg>
</template>

<style scoped>
.db { width: 100%; height: auto; font-variant-numeric: tabular-nums; }
text { fill: #0b0b0b; font-size: 13px; }
.legend text { fill: #52514e; font-size: 12px; }
.name { text-anchor: end; font-size: 13px; }
.tick { fill: #898781; font-size: 12px; text-anchor: middle; }
.grid { stroke: #e1e0d9; }
.link { stroke: #c3c2b7; stroke-width: 2; }
.mean { fill: #2a78d6; stroke: #fff; stroke-width: 2; }
.best { fill: #eb6834; stroke: #fff; stroke-width: 2; }
.exp { stroke: #24414c; stroke-width: 2; stroke-dasharray: 5 3; }
</style>
