<script setup>
import { ref, reactive } from 'vue';
import html2canvas from 'html2canvas';
import QRCode from 'qrcode';

const props = defineProps({
  result: {
    type: Object,
    required: true,
  },
  nickname: {
    type: String,
    required: true,
  },
});

const reportRef = ref(null);
const posterImageUrl = ref('');
const isGenerating = ref(false);
const emits = defineEmits(['retry']);
const currentDate = new Date().toLocaleDateString();

// 生成海报图（包含二维码）
const generatePoster = async () => {
  if (!reportRef.value) return;

  isGenerating.value = true;

  try {
    // 生成二维码
    const websiteUrl = window.location.origin;
    const qrCodeDataUrl = await QRCode.toDataURL(websiteUrl, {
      width: 200,
      margin: 2,
      color: {
        dark: '#000000',
        light: '#FFFFFF',
      },
    });

    // 创建一个临时的海报容器
    const tempContainer = document.createElement('div');
    tempContainer.style.cssText = `
      position: fixed;
      top: -9999px;
      left: -9999px;
      width: 800px;
      background: #fdfbf7;
      padding: 40px;
      border-radius: 20px;
      box-sizing: border-box;
      z-index: -1;
    `;

    // 创建海报内容
    const posterHTML = `
      <div style="background: #fdfbf7; padding: 40px; border-radius: 20px; max-width: 800px; width: 100%;">
        <!-- 诊断报告内容 -->
        <div style="background: #fdfbf7; width: 100%; position: relative;">
          <!-- Stamp -->
          <div style="position: absolute; top: 40px; right: 40px; width: 96px; height: 96px; border: 4px solid #dc2626; border-radius: 50%; display: flex; align-items: center; justify-content: center; opacity: 0.5; transform: rotate(12deg); pointer-events: none;">
            <span style="color: #dc2626; font-weight: bold; font-size: 18px; text-transform: uppercase; letter-spacing: 2px;">已确诊</span>
          </div>

          <!-- Header -->
          <div style="border-bottom: 4px solid #1f2937; padding-bottom: 24px; margin-bottom: 32px; text-align: center;">
            <h1 style="font-size: 32px; font-weight: 900; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 8px; line-height: 1.2;">
              互联网精神状态<br />临床诊断报告书
            </h1>
            <div style="display: flex; justify-content: space-between; font-size: 14px; font-weight: bold; margin-top: 16px; text-transform: uppercase;">
              <span>编号: NO.${Math.floor(Math.random() * 100000)}</span>
              <span>日期: ${currentDate}</span>
            </div>
          </div>

          <!-- Patient Info -->
          <div style="margin-bottom: 32px; display: flex; align-items: baseline; border-bottom: 2px solid #d1d5db; padding-bottom: 16px;">
            <span style="font-weight: bold; font-size: 18px; margin-right: 16px;">患者姓名:</span>
            <span style="font-size: 24px; font-family: 'Courier New', Courier, monospace; font-style: italic; color: #1d4ed8; border-bottom: 2px solid #1d4ed8; padding: 0 16px;">${
              props.nickname
            }</span>
          </div>

          <!-- Diagnosis Result -->
          <div style="margin-bottom: 32px;">
            <h2 style="font-weight: bold; font-size: 20px; margin-bottom: 12px; display: flex; align-items: center;">
              <span style="background: #000; color: white; padding: 4px 8px; margin-right: 8px; font-size: 14px;">01</span> 核心诊断
            </h2>
            <div style="background: #f3f4f6; padding: 16px; border-left: 4px solid #000;">
              <p style="font-size: 24px; font-weight: bold; color: #dc2626; line-height: 1.2;">
                ${props.result.diagnosis}
              </p>
            </div>
          </div>

          <!-- Symptoms -->
          <div style="margin-bottom: 32px;">
            <h2 style="font-weight: bold; font-size: 20px; margin-bottom: 12px; display: flex; align-items: center;">
              <span style="background: #000; color: white; padding: 4px 8px; margin-right: 8px; font-size: 14px;">02</span> 临床表现
            </h2>
            <ul style="list-style-type: disc; list-style-position: inside; space-y: 8px; font-size: 18px;">
              ${props.result.symptoms
                .map((symptom) => `<li>${symptom}</li>`)
                .join('')}
            </ul>
          </div>

          <!-- Causes -->
          <div style="margin-bottom: 32px;">
            <h2 style="font-weight: bold; font-size: 20px; margin-bottom: 12px; display: flex; align-items: center;">
              <span style="background: #000; color: white; padding: 4px 8px; margin-right: 8px; font-size: 14px;">03</span> 病因分析
            </h2>
            <p style="font-size: 18px; font-style: italic; color: #374151;">
              ${props.result.cause}
            </p>
          </div>

          <!-- Prescription -->
          <div style="margin-bottom: 32px;">
            <h2 style="font-weight: bold; font-size: 20px; margin-bottom: 12px; display: flex; align-items: center;">
              <span style="background: #000; color: white; padding: 4px 8px; margin-right: 8px; font-size: 14px;">04</span> 治疗方案
            </h2>
            <div style="border: 2px dashed #9ca3af; padding: 16px; border-radius: 8px; background: #fefce8;">
              <p style="font-size: 18px; font-weight: 500; color: #1f2937;">
                ${props.result.prescription}
              </p>
            </div>
          </div>

          <!-- 二维码区域 -->
          <div style="margin-top: 40px; padding-top: 30px; border-top: 2px dashed #ccc; text-align: center;">
            <h3 style="font-size: 18px; font-weight: bold; margin-bottom: 15px; color: #333;">
              扫描二维码，获取你的专属诊断报告
            </h3>
            <img src="${qrCodeDataUrl}" alt="网站二维码" style="width: 150px; height: 150px; border: 8px solid white; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);" />
            <div style="margin-top: 20px; text-align: center; font-style: italic; color: #666; font-size: 14px;">
              📱 长按分享你的诊断报告
            </div>
          </div>
        </div>
      </div>
    `;

    tempContainer.innerHTML = posterHTML;
    document.body.appendChild(tempContainer);

    // 等待图片加载
    await new Promise((resolve) => setTimeout(resolve, 500));

    // 生成截图
    const canvas = await html2canvas(tempContainer, {
      backgroundColor: '#fdfbf7',
      scale: 2, // 高清截图
      useCORS: true,
      allowTaint: false,
      logging: false,
    });

    // 将图片URL保存并显示
    posterImageUrl.value = canvas.toDataURL('image/png', 1.0);

    // 清理临时容器
    document.body.removeChild(tempContainer);
  } catch (error) {
    console.error('生成海报失败:', error);
    alert('生成海报失败，请重试或手动截图');
  } finally {
    isGenerating.value = false;
  }
};

// 下载海报
const downloadPoster = () => {
  if (posterImageUrl.value) {
    const link = document.createElement('a');
    link.download = `精神状态诊断报告_${
      props.nickname
    }_${new Date().getTime()}.png`;
    link.href = posterImageUrl.value;
    link.click();
  }
};

// 重新生成海报
const regeneratePoster = () => {
  posterImageUrl.value = '';
  generatePoster();
};
</script>

<template>
  <div
    class="min-h-screen bg-gray-200 p-4 md:p-8 flex items-center justify-center font-serif text-gray-900"
  >
    <div
      ref="reportRef"
      class="bg-[#fdfbf7] w-full max-w-2xl shadow-2xl border-4 border-gray-800 p-6 md:p-10 relative rotate-1 transform transition-transform hover:rotate-0 duration-500"
    >
      <!-- Stamp -->
      <div
        class="absolute top-10 right-10 w-32 h-32 border-4 border-red-600 rounded-full flex items-center justify-center opacity-50 transform rotate-12 pointer-events-none"
      >
        <span class="text-red-600 font-bold text-xl uppercase tracking-widest"
          >已确诊</span
        >
      </div>

      <!-- Header -->
      <div class="border-b-4 border-gray-800 pb-6 mb-8 text-center">
        <h1
          class="text-3xl md:text-4xl font-black tracking-widest uppercase mb-2"
        >
          互联网精神状态<br />临床诊断报告书
        </h1>
        <div class="flex justify-between text-sm font-bold mt-4 uppercase">
          <span>编号: NO.{{ Math.floor(Math.random() * 100000) }}</span>
          <span>日期: {{ currentDate }}</span>
        </div>
      </div>

      <!-- Patient Info -->
      <div class="mb-8 flex items-baseline border-b-2 border-gray-300 pb-4">
        <span class="font-bold text-lg mr-4">患者姓名:</span>
        <span
          class="text-2xl font-handwriting text-blue-700 border-b-2 border-blue-700 px-4"
          >{{ nickname }}</span
        >
      </div>

      <!-- Diagnosis Result -->
      <div class="mb-8">
        <h2 class="font-bold text-xl mb-3 flex items-center">
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">01</span>
          核心诊断
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
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">02</span>
          临床表现
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
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">03</span>
          病因分析
        </h2>
        <p class="text-lg italic text-gray-700">
          {{ result.cause }}
        </p>
      </div>

      <!-- Prescription -->
      <div class="mb-8">
        <h2 class="font-bold text-xl mb-3 flex items-center">
          <span class="bg-black text-white px-2 py-1 mr-2 text-sm">04</span>
          治疗方案
        </h2>
        <div
          class="border-2 border-dashed border-gray-400 p-4 rounded bg-yellow-50"
        >
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
            @click="generatePoster"
            class="bg-black text-white px-6 py-2 font-bold hover:bg-gray-800 transition-colors"
          >
            📸 生成诊断书海报
          </button>
          <button
            @click="emits('retry')"
            class="border-2 border-black px-6 py-2 font-bold hover:bg-gray-100 transition-colors"
          >
            🔄 再测一次
          </button>
        </div>
      </div>
      <div class="flex justify-center items-center mt-2 flex-col">
        <img
          src="https://seeyou.cool/storage/img?key=WHlEbHk0MjkwYTY0ZUZXdExxYnFMZEN4aEFjRGYzK1JZNDl0QkE9PQ=="
          alt=""
        />
        <span class="font-bold text-xl">瞅一眼作者</span>
      </div>
    </div>
  </div>

  <!-- 生成的海报图片展示 -->
  <div
    v-if="posterImageUrl"
    class="fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center z-50 p-4"
  >
    <div class="bg-white max-w-4xl w-full rounded-2xl shadow-2xl relative">
      <div class="p-6">
        <h2 class="text-2xl font-bold text-center mb-4 text-gray-800">
          你的诊断书海报已生成
        </h2>

        <!-- 生成的图片 -->
        <div class="flex justify-center mb-6">
          <img
            :src="posterImageUrl"
            alt="诊断书海报"
            class="max-w-full max-h-[70vh] rounded-lg shadow-lg border-4 border-white"
          />
        </div>

        <!-- 操作按钮 -->
        <div class="flex justify-center space-x-4">
          <button
            @click="posterImageUrl = ''"
            class="bg-gray-600 text-white px-6 py-3 rounded-lg font-bold hover:bg-gray-700 transition-colors flex items-center"
          >
            ❌ 关闭
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- 生成中提示 -->
  <div
    v-if="isGenerating"
    class="fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center z-50"
  >
    <div class="bg-white p-8 rounded-2xl shadow-2xl text-center">
      <div
        class="animate-spin rounded-full h-16 w-16 border-b-2 border-blue-600 mx-auto mb-4"
      ></div>
      <p class="text-xl font-bold text-gray-800">正在生成海报...</p>
      <p class="text-gray-600 mt-2">请稍等片刻</p>
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
