<script setup>
import { ref } from 'vue';

const emit = defineEmits(['start']);
const nickname = ref('');

const startDiagnosis = () => {
  if (!nickname.value.trim()) {
    alert('请输入您的网络昵称（随便填一个也行）');
    return;
  }
  emit('start', nickname.value);
};
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-black text-white overflow-hidden relative">
    <!-- Background Animation (Abstract) -->
    <div class="absolute inset-0 opacity-20 pointer-events-none">
      <div class="animate-pulse absolute top-10 left-10 text-xs text-green-500">SYSTEM READY</div>
      <div class="animate-bounce absolute bottom-20 right-20 text-xs text-red-500">WAITING FOR INPUT</div>
      <!-- Add more random floating text or elements here if needed -->
    </div>

    <div class="z-10 text-center space-y-8 p-4">
      <h1 class="text-4xl md:text-6xl font-bold tracking-tighter mb-4 glitch-text">
        当代网友<br/>精神状态诊断
      </h1>
      
      <p class="text-gray-400 text-sm md:text-base max-w-md mx-auto">
        警告：本诊断结果可能过于真实，请在家长（或猫猫）陪同下观看。
      </p>

      <div class="flex flex-col items-center space-y-4 w-full max-w-xs mx-auto">
        <input 
          v-model="nickname"
          type="text" 
          placeholder="请输入您的网络昵称" 
          class="w-full px-4 py-3 bg-gray-800 border border-gray-700 rounded-lg focus:outline-none focus:border-red-500 text-center text-lg placeholder-gray-600 transition-colors"
          @keyup.enter="startDiagnosis"
        />
        
        <button 
          @click="startDiagnosis"
          class="w-full py-4 bg-red-600 hover:bg-red-700 text-white font-bold text-xl rounded-lg shadow-[0_0_20px_rgba(220,38,38,0.5)] hover:shadow-[0_0_30px_rgba(220,38,38,0.8)] transform hover:scale-105 transition-all duration-200"
        >
          点击诊断
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.glitch-text {
  text-shadow: 2px 2px 0px #ff0000, -2px -2px 0px #00ff00;
  animation: glitch 1s infinite alternate;
}

@keyframes glitch {
  0% { text-shadow: 2px 2px 0px #ff0000, -2px -2px 0px #00ff00; }
  25% { text-shadow: -2px 2px 0px #ff0000, 2px -2px 0px #00ff00; }
  50% { text-shadow: 2px -2px 0px #ff0000, -2px 2px 0px #00ff00; }
  75% { text-shadow: -2px -2px 0px #ff0000, 2px 2px 0px #00ff00; }
  100% { text-shadow: 2px 2px 0px #ff0000, -2px -2px 0px #00ff00; }
}
</style>
