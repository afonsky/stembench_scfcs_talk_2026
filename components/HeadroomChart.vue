<script setup lang="ts">
// Per-task raw scores (mean of 3 retained high-effort runs) against the
// baseline b and expert e anchors. Source: draft paper, Table "Per-Model Results".
const models = [
  'Gemini 3.7 Flash', 'Gemini 3.1 Pro', 'Claude Opus 5', 'DeepSeek V4 Pro',
  'Qwen3.8 2.4T', 'GLM 5.2', 'MiMo V2.5', 'MiniMax M3',
]

const tasks = [
  { d: 'HEP', n: 'New-physics search', b: 0.327, e: 0.912, m: [0.31, 0.93, 0.53, 0.83, 0.54, 0.62, 0.00, 0.38] },
  { d: 'HEP', n: 'Higgs uncertainty', b: 0.400, e: 0.687, m: [0.41, 0.43, 0.48, 0.39, 0.44, 0.28, 0.14, 0.13] },
  { d: 'HEP', n: 'Top tagging', b: 0.063, e: 0.884, m: [0.56, 0.65, 0.94, 0.89, 0.93, 0.60, 0.59, 0.56] },
  { d: 'HEP', n: 'Dark Machines', b: 0.003, e: 0.221, m: [0.22, 0.29, 0.25, 0.28, 0.22, 0.32, 0.18, 0.20] },
  { d: 'Astro', n: 'Stokes, simulated', b: 0.009, e: 0.624, m: [0.01, 0.78, 0.57, 0.25, 0.00, 0.00, 0.43, 0.11] },
  { d: 'Astro', n: 'Stokes, real (Hinode)', b: 0.000, e: 0.598, m: [0.12, 0.82, 0.09, 0.00, 0.00, 0.00, 0.14, 0.00] },
  { d: 'Astro', n: 'SN Ia classification', b: 0.421, e: 0.849, m: [0.50, 0.91, 0.94, 0.90, 0.93, 0.60, 0.93, 0.93] },
  { d: 'Neuro', n: 'ASD classification', b: 0.000, e: 0.312, m: [0.09, 0.00, 0.00, 0.10, 0.10, 0.29, 0.08, 0.19] },
  { d: 'Neuro', n: 'NMM family id.', b: 0.000, e: 0.900, m: [0.00, 1.00, 0.00, 0.51, 0.56, 0.00, 0.37, 0.02] },
  { d: 'Neuro', n: 'NMM digital twins', b: 0.100, e: 0.715, m: [0.10, 0.24, 0.31, 0.14, 0.10, 0.03, 0.00, 0.10] },
  { d: 'Neuro', n: 'NMM param. recovery', b: 0.125, e: 0.515, m: [0.12, 0.18, 0.42, 0.12, 0.12, 0.08, 0.02, 0.12] },
  { d: 'Neuro', n: 'BOLD deconvolution', b: 0.012, e: 0.353, m: [0.00, 0.19, 0.00, 0.00, 0.02, 0.00, 0.00, 0.06] },
]

const W = 880
const X0 = 236 // plot left
const X1 = 660 // plot right
const TOP = 46
const ROW = 23
const GAP = 9
const x = (v: number) => X0 + v * (X1 - X0)

let y = TOP
let prev = ''
const rows = tasks.map((t) => {
  if (prev && t.d !== prev) y += GAP
  const first = t.d !== prev
  prev = t.d
  const best = t.m.indexOf(Math.max(...t.m))
  const row = { ...t, y: y + ROW / 2, first, best, above: t.m[best] > t.e }
  y += ROW
  return row
})
const H = y + 26
const ticks = [0, 0.25, 0.5, 0.75, 1]
const fmt = (v: number) => v.toFixed(2)
</script>

<template>
  <svg :viewBox="`0 0 ${W} ${H}`" class="headroom" role="img"
       aria-label="Per-task scores of eight models against baseline and expert anchors">
    <!-- legend -->
    <g class="legend" transform="translate(0,14)">
      <circle cx="6" cy="0" r="4" class="dot" />
      <text x="16" y="4">each model (mean of 3 runs)</text>
      <circle cx="226" cy="0" r="6" class="best" />
      <text x="238" y="4">best model</text>
      <rect x="330" y="-6" width="34" height="12" rx="2" class="band" />
      <text x="372" y="4">baseline b → expert e</text>
      <line x1="560" y1="-8" x2="560" y2="8" class="expert" />
      <text x="568" y="4">expert anchor</text>
      <text x="700" y="4" class="ok">✓</text>
      <text x="714" y="4">best mean above e</text>
    </g>

    <!-- grid -->
    <g v-for="t in ticks" :key="t">
      <line :x1="x(t)" :x2="x(t)" :y1="TOP - 4" :y2="H - 22" class="grid" />
      <text :x="x(t)" :y="H - 6" class="tick">{{ t }}</text>
    </g>

    <g v-for="r in rows" :key="r.n">
      <text v-if="r.first" x="0" :y="r.y + 5" class="domain">{{ r.d }}</text>
      <text x="52" :y="r.y + 5" class="task">{{ r.n }}</text>
      <rect :x="x(r.b)" :y="r.y - 6" :width="x(r.e) - x(r.b)" height="12" rx="2" class="band">
        <title>{{ r.n }}: baseline {{ r.b.toFixed(3) }} → expert {{ r.e.toFixed(3) }}</title>
      </rect>
      <line :x1="x(r.b)" :x2="x(r.b)" :y1="r.y - 8" :y2="r.y + 8" class="baseline" />
      <line :x1="x(r.e)" :x2="x(r.e)" :y1="r.y - 9" :y2="r.y + 9" class="expert" />
      <circle v-for="(v, i) in r.m" :key="i" v-show="i !== r.best"
              :cx="x(v)" :cy="r.y" r="4" class="dot">
        <title>{{ models[i] }}: {{ fmt(v) }}</title>
      </circle>
      <circle :cx="x(r.m[r.best])" :cy="r.y" r="6" class="best">
        <title>{{ models[r.best] }}: {{ fmt(r.m[r.best]) }} (best)</title>
      </circle>
      <text :x="X1 + 16" :y="r.y + 5" class="val">{{ fmt(r.m[r.best]) }}</text>
      <text :x="X1 + 56" :y="r.y + 5" class="who">{{ models[r.best] }}</text>
      <text v-if="r.above" :x="W - 4" :y="r.y + 5" class="ok" text-anchor="end">✓</text>
    </g>
  </svg>
</template>

<style scoped>
.headroom { width: 100%; height: auto; font-variant-numeric: tabular-nums; }
text { fill: #0b0b0b; font-size: 14px; }
.legend text { font-size: 13px; fill: #52514e; }
.domain { font-size: 12px; fill: #898781; letter-spacing: 0.06em; text-transform: uppercase; }
.task { font-size: 14px; }
.tick { font-size: 12px; fill: #898781; text-anchor: middle; }
.val { font-size: 14px; font-weight: 700; }
.who { font-size: 13px; fill: #52514e; }
.ok { fill: #0b0b0b; font-weight: 700; font-size: 15px; }
.grid { stroke: #e1e0d9; stroke-width: 1; }
.band { fill: #d5e3e8; }
.baseline { stroke: #898781; stroke-width: 1.5; stroke-dasharray: 3 2; }
.expert { stroke: #24414c; stroke-width: 2.5; }
.dot { fill: #a9a79f; stroke: #ffffff; stroke-width: 1.5; }
.best { fill: #2a78d6; stroke: #ffffff; stroke-width: 2; }
</style>
