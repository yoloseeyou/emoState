<script setup>
import { ref, computed } from 'vue';
import LandingPage from './components/LandingPage.vue';
import ScanningPage from './components/ScanningPage.vue';
import ReportPage from './components/ReportPage.vue';
import { diagnoses, symptoms, causes, prescriptions } from './data/corpus';

const STAGES = {
  LANDING: 'landing',
  SCANNING: 'scanning',
  REPORT: 'report'
};

const currentStage = ref(STAGES.LANDING);
const nickname = ref('');
const diagnosisResult = ref(null);

const startDiagnosis = (name) => {
  nickname.value = name;
  currentStage.value = STAGES.SCANNING;
};

const generateDiagnosis = () => {
  // Randomly select items from corpus
  const diagnosis = diagnoses[Math.floor(Math.random() * diagnoses.length)];
  
  // Select 3 random symptoms
  const selectedSymptoms = [];
  const symptomsCopy = [...symptoms];
  for (let i = 0; i < 3; i++) {
    if (symptomsCopy.length === 0) break;
    const randomIndex = Math.floor(Math.random() * symptomsCopy.length);
    selectedSymptoms.push(symptomsCopy.splice(randomIndex, 1)[0]);
  }

  const cause = causes[Math.floor(Math.random() * causes.length)];
  const prescription = prescriptions[Math.floor(Math.random() * prescriptions.length)];

  diagnosisResult.value = {
    diagnosis,
    symptoms: selectedSymptoms,
    cause,
    prescription
  };

  currentStage.value = STAGES.REPORT;
};

const resetDiagnosis = () => {
  currentStage.value = STAGES.LANDING;
  nickname.value = '';
  diagnosisResult.value = null;
};
</script>

<template>
  <div class="antialiased">
    <Transition name="fade" mode="out-in">
      <LandingPage 
        v-if="currentStage === STAGES.LANDING" 
        @start="startDiagnosis" 
      />
      <ScanningPage 
        v-else-if="currentStage === STAGES.SCANNING" 
        @finish="generateDiagnosis" 
      />
      <ReportPage 
        v-else-if="currentStage === STAGES.REPORT" 
        :result="diagnosisResult" 
        :nickname="nickname"
        @retry="resetDiagnosis"
      />
    </Transition>
  </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
