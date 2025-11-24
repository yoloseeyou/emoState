<script setup>
import { reactive, computed } from 'vue';

const props = defineProps({
  questions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['complete', 'back']);

const responses = reactive({});

const allAnswered = computed(() =>
  props.questions.every((question) => Boolean(responses[question.id]))
);

const handleSubmit = () => {
  if (!allAnswered.value) return;

  const answerList = props.questions.map((question) => ({
    id: question.id,
    prompt: question.prompt,
    answer: responses[question.id],
  }));

  emit('complete', answerList);
};
</script>

<template>
  <div
    class="min-h-screen bg-slate-950 text-white flex items-center justify-center p-4"
  >
    <div class="w-full max-w-3xl space-y-8">
      <div class="text-center space-y-2">
        <p class="text-sm uppercase tracking-[0.3em] text-rose-400">
          step 02 / 03
        </p>
        <h2 class="text-3xl font-black">情绪问诊表</h2>
        <p class="text-slate-400 text-sm">
          快速回答几个灵魂追问，我们的诊断器材就能精准瞄准你的精神状态。
        </p>
      </div>

      <div class="space-y-6 max-h-[60vh] overflow-y-auto pr-2 custom-scroll">
        <div
          v-for="question in questions"
          :key="question.id"
          class="bg-slate-900/80 border border-slate-800 rounded-2xl p-6 shadow-lg"
        >
          <h3 class="text-lg font-semibold mb-4 text-rose-200">
            {{ question.prompt }}
          </h3>

          <div class="grid gap-3 md:grid-cols-2">
            <button
              v-for="option in question.options"
              :key="option"
              type="button"
              class="px-4 py-3 text-left rounded-xl border transition-all duration-150"
              :class="[
                responses[question.id] === option
                  ? 'bg-rose-500/20 border-rose-400 text-white shadow-[0_0_15px_rgba(244,63,94,0.4)]'
                  : 'border-slate-700 text-slate-300 hover:border-rose-500 hover:text-white',
              ]"
              @click="responses[question.id] = option"
            >
              {{ option }}
            </button>
          </div>
        </div>
      </div>

      <div class="flex flex-col sm:flex-row gap-4">
        <button
          type="button"
          class="flex-1 py-3 rounded-full border border-slate-700 text-slate-300 hover:border-slate-500 hover:text-white transition-colors"
          @click="emit('back')"
        >
          ← 返回填写昵称
        </button>

        <button
          type="button"
          class="flex-1 py-3 rounded-full font-semibold transition-all duration-200"
          :class="[
            allAnswered
              ? 'bg-rose-500 hover:bg-rose-600 text-white shadow-[0_0_25px_rgba(244,63,94,0.5)]'
              : 'bg-slate-700 text-slate-400 cursor-not-allowed',
          ]"
          :disabled="!allAnswered"
          @click="handleSubmit"
        >
          提交问卷并开始扫描 →
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.custom-scroll::-webkit-scrollbar {
  width: 6px;
}
.custom-scroll::-webkit-scrollbar-thumb {
  background: rgba(248, 113, 113, 0.5);
  border-radius: 999px;
}
</style>

