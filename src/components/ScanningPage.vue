<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { scanningPhrases } from '../data/corpus';

const emit = defineEmits(['finish']);

const currentPhrase = ref('正在初始化扫描程序...');
const progress = ref(0);

let intervalId = null;
let phraseIntervalId = null;

onMounted(() => {
  // Progress bar animation
  intervalId = setInterval(() => {
    if (progress.value < 100) {
      progress.value += Math.random() * 5;
      if (progress.value > 100) progress.value = 100;
    } else {
      clearInterval(intervalId);
      clearInterval(phraseIntervalId);
      setTimeout(() => {
        emit('finish');
      }, 500);
    }
  }, 100);

  // Random phrase switching
  phraseIntervalId = setInterval(() => {
    const randomIndex = Math.floor(Math.random() * scanningPhrases.length);
    currentPhrase.value = scanningPhrases[randomIndex];
  }, 800);
});

onUnmounted(() => {
  clearInterval(intervalId);
  clearInterval(phraseIntervalId);
});
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-black text-green-500 font-mono p-4">
    <div class="w-full max-w-md space-y-8">
      <!-- Scanner Visual -->
      <div class="relative h-64 border-2 border-green-500 rounded-lg overflow-hidden bg-gray-900">
        <div class="absolute inset-0 grid grid-cols-12 grid-rows-12 gap-1 opacity-20">
          <div v-for="i in 144" :key="i" class="bg-green-500/30"></div>
        </div>
        
        <!-- Scanning Line -->
        <div class="absolute top-0 left-0 w-full h-2 bg-green-400 shadow-[0_0_10px_#4ade80] animate-scan"></div>
        
        <!-- Central Icon/Animation -->
        <div class="absolute inset-0 flex items-center justify-center">
          <div class="text-6xl animate-pulse">🧠</div>
        </div>
      </div>

      <!-- Text Output -->
      <div class="text-center space-y-2">
        <div class="text-xl font-bold min-h-[3rem] flex items-center justify-center">
          {{ currentPhrase }}
        </div>
        <div class="w-full bg-gray-800 h-4 rounded-full overflow-hidden border border-green-900">
          <div 
            class="h-full bg-green-600 transition-all duration-100 ease-out"
            :style="{ width: `${progress}%` }"
          ></div>
        </div>
        <div class="text-right text-xs opacity-70">{{ Math.floor(progress) }}%</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.animate-scan {
  animation: scan 2s linear infinite;
}

@keyframes scan {
  0% { top: 0%; opacity: 0.5; }
  50% { opacity: 1; }
  100% { top: 100%; opacity: 0.5; }
}
</style>
