<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  result: {
    type: Object,
    required: true
  },
  nickname: {
    type: String,
    required: true
  }
});

const reportRef = ref(null);

const currentDate = new Date().toLocaleDateString();

// Placeholder for screenshot function
const downloadScreenshot = () => {
  alert('截图功能开发中... 请手动截图分享！(按 Win+Shift+S)');
};
</script>

<template>
  <div class="min-h-screen bg-gray-200 p-4 md:p-8 flex items-center justify-center font-serif text-gray-900">
    <div ref="reportRef" class="bg-[#fdfbf7] w-full max-w-2xl shadow-2xl border-4 border-gray-800 p-6 md:p-10 relative rotate-1 transform transition-transform hover:rotate-0 duration-500">
      
      <!-- Stamp -->
      <div class="absolute top-10 right-10 w-32 h-32 border-4 border-red-600 rounded-full flex items-center justify-center opacity-50 transform rotate-12 pointer-events-none">
        <span class="text-red-600 font-bold text-xl uppercase tracking-widest">已确诊</span>
      </div>

      <!-- Header -->
      <div class="border-b-4 border-gray-800 pb-6 mb-8 text-center">
        <h1 class="text-3xl md:text-4xl font-black tracking-widest uppercase mb-2">
          互联网精神状态<br/>临床诊断报告书
        </h1>
        <div class="flex justify-between text-sm font-bold mt-4 uppercase">
          <span>编号: NO.{{ Math.floor(Math.random() * 100000) }}</span>
          <span>日期: {{ currentDate }}</span>
        </div>
      </div>

      <!-- Patient Info -->
      <div class="mb-8 flex items-baseline border-b-2 border-gray-300 pb-4">
        <span class="font-bold text-lg mr-4">患者姓名:</span>
        <span class="text-2xl font-handwriting text-blue-700 border-b-2 border-blue-700 px-4">{{ nickname }}</span>
      </div>

      <!-- Diagnosis Result -->
      <div class="mb-8">
        <h2 class="font-bold text-xl mb-3 flex items-center">
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">01</span> 核心诊断
        </h2>
        <div class="bg-gray-100 p-4 border-l-4 border-black">
          <p class="text-2xl md:text-3xl font-bold text-red-600 leading-tight">
            {{ result.diagnosis }}
          </p>
        </div>
      </div>

      <!-- Symptoms -->
      <div class="mb-8">
        <h2 class="font-bold text-xl mb-3 flex items-center">
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">02</span> 临床表现
        </h2>
        <ul class="list-disc list-inside space-y-2 text-lg">
          <li v-for="(symptom, index) in result.symptoms" :key="index">
            {{ symptom }}
          </li>
        </ul>
      </div>

      <!-- Causes -->
      <div class="mb-8">
        <h2 class="font-bold text-xl mb-3 flex items-center">
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">03</span> 病因分析
        </h2>
        <p class="text-lg italic text-gray-700">
          {{ result.cause }}
        </p>
      </div>

      <!-- Prescription -->
      <div class="mb-8">
        <h2 class="font-bold text-xl mb-3 flex items-center">
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">04</span> 治疗方案
        </h2>
        <div class="border-2 border-dashed border-gray-400 p-4 rounded bg-yellow-50">
          <p class="text-lg font-medium text-gray-800">
            {{ result.prescription }}
          </p>
        </div>
      </div>

      <!-- Footer -->
      <div class="text-center mt-12 pt-6 border-t-4 border-gray-800">
        <p class="text-xs font-bold tracking-widest uppercase mb-4">
          此报告由「高维生物观测站」自动生成 · 仅供娱乐 · 如有雷同纯属巧合
        </p>
        <div class="flex justify-center space-x-4 no-print">
          <button 
            @click="downloadScreenshot"
            class="bg-black text-white px-6 py-2 font-bold hover:bg-gray-800 transition-colors"
          >
            📸 生成诊断书截图
          </button>
          <button 
            @click="$emit('retry')"
            class="border-2 border-black px-6 py-2 font-bold hover:bg-gray-100 transition-colors"
          >
            🔄 再测一次
          </button>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>
.font-handwriting {
  font-family: 'Courier New', Courier, monospace; /* Fallback, ideally use a handwriting font */
  font-style: italic;
}

@media print {
  .no-print {
    display: none;
  }
}
</style>
