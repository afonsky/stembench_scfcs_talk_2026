<script setup lang="ts">
// Domain means (unweighted mean of task means) and number of tasks led.
// Source: draft paper, Table "Per-Model Results".
const models = [
  ['Gemini', '3.7 Flash'], ['Gemini', '3.1 Pro'], ['Claude', 'Opus 5'], ['DeepSeek', 'V4 Pro'],
  ['Qwen3.8', '2.4T'], ['GLM', '5.2'], ['MiMo', 'V2.5'], ['MiniMax', 'M3'],
]
const rows = [
  { d: 'HEP', v: [0.37, 0.57, 0.55, 0.60, 0.53, 0.46, 0.23, 0.32] },
  { d: 'Astro', v: [0.21, 0.84, 0.53, 0.38, 0.31, 0.20, 0.50, 0.34] },
  { d: 'Neuro', v: [0.06, 0.32, 0.15, 0.18, 0.18, 0.08, 0.09, 0.10] },
]
const wins = [0, 5, 5, 0, 0, 2, 0, 0]

// single-hue sequential ramp, light -> dark
const ramp = ['#e6f0fc', '#cde2fb', '#b7d3f6', '#9ec5f4', '#86b6ef', '#6da7ec',
  '#5598e7', '#3987e5', '#2a78d6', '#256abf', '#1c5cab', '#184f95']
const idx = (v: number) => Math.min(ramp.length - 1, Math.round((v / 0.85) * (ramp.length - 1)))
const cell = (v: number) => ({
  background: ramp[idx(v)],
  color: idx(v) >= 7 ? '#ffffff' : '#0b0b0b',
})
const isMax = (r: { v: number[] }, i: number) => r.v[i] === Math.max(...r.v)
</script>

<template>
  <table class="heat">
    <thead>
      <tr>
        <th />
        <th v-for="m in models" :key="m[1]" class="mh">{{ m[0] }}<br>{{ m[1] }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="r in rows" :key="r.d">
        <th class="rh">{{ r.d }}</th>
        <td v-for="(v, i) in r.v" :key="i" :style="cell(v)" :class="{ max: isMax(r, i) }"
            :title="`${models[i].join(' ')} — ${r.d} mean ${v.toFixed(2)}`">
          {{ v.toFixed(2) }}
        </td>
      </tr>
      <tr class="wins">
        <th class="rh">tasks led</th>
        <td v-for="(w, i) in wins" :key="i" :class="{ strong: w > 0 }">{{ w || '·' }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
.heat { border-collapse: separate; border-spacing: 2px; font-variant-numeric: tabular-nums; width: 100%; }
.heat th, .heat td { padding: 0; border: none; }
.mh { font-size: 11px; line-height: 1.15; font-weight: 400; color: #52514e; text-align: center; padding-bottom: 4px !important; }
.rh { font-size: 13px; font-weight: 400; color: #52514e; text-align: right; padding-right: 8px !important; white-space: nowrap; }
.heat td { font-size: 15px; text-align: center; height: 40px; border-radius: 4px; min-width: 50px; }
.heat td.max { font-weight: 700; outline: 2.5px solid #0b0b0b; outline-offset: -2.5px; }
.wins td { background: transparent; color: #898781; height: 26px; font-size: 14px; }
.wins td.strong { color: #0b0b0b; font-weight: 700; }
</style>
