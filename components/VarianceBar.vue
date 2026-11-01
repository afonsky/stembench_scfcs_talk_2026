<script setup lang="ts">
// Two-way sum-of-squares decomposition of the 96 task-model means (draft paper, Results).
const parts = [
  { k: 'task', label: 'Which task', v: 60.5, color: '#2a78d6', ink: '#ffffff' },
  { k: 'model', label: 'Which model', v: 11.5, color: '#eb6834', ink: '#ffffff' },
  { k: 'rest', label: 'Task × model + run noise', v: 28.0, color: '#c3c2b7', ink: '#0b0b0b' },
]
</script>

<template>
  <div class="vb">
    <div class="labels">
      <div v-for="p in parts" :key="p.k" :style="{ flexBasis: p.v + '%' }" class="lab">
        {{ p.label }}
      </div>
    </div>
    <div class="bar" role="img" aria-label="Variance shares: task 60.5%, model 11.5%, interaction and noise 28%">
      <div v-for="p in parts" :key="p.k" :style="{ flexBasis: p.v + '%', background: p.color, color: p.ink }"
           class="seg" :title="`${p.label}: ${p.v}%`">
        {{ p.v }}%
      </div>
    </div>
  </div>
</template>

<style scoped>
.vb { font-variant-numeric: tabular-nums; }
.labels, .bar { display: flex; gap: 2px; }
.lab { font-size: 13px; color: #52514e; padding: 0 2px 3px; white-space: nowrap; overflow: visible; }
.seg { height: 30px; display: flex; align-items: center; justify-content: center; font-size: 15px; font-weight: 700; }
.seg:first-child { border-radius: 4px 0 0 4px; }
.seg:last-child { border-radius: 0 4px 4px 0; }
</style>
