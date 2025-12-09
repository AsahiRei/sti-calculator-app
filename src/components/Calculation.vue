<template>
  <div class="mt-4 bg-white border border-gray-300 rounded-md p-4">
    <h1 class="font-semibold text-md md:text-lg lg:text-xl text-gray-700">
      Calculate GWA
    </h1>
    <div class="mt-2 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <div>
        <label for="prelims" class="font-semibold text-gray-700"
          >Prelims (20%)</label
        >
        <input
          id="prelims"
          step="0.01"
          type="number"
          placeholder="0.00"
          v-model="prelims"
          class="w-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition py-1 px-2 rounded-md border border-gray-300"
        />
      </div>
      <div>
        <label for="midterms" class="font-semibold text-gray-700"
          >Midterms (20%)</label
        >
        <input
          id="midterms"
          step="0.01"
          type="number"
          placeholder="0.00"
          v-model="midterms"
          class="w-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition py-1 px-2 rounded-md border border-gray-300"
        />
      </div>
      <div>
        <label for="prefinal" class="font-semibold text-gray-700"
          >Prefinal (20%)</label
        >
        <input
          id="prefinal"
          step="0.01"
          type="number"
          placeholder="0.00"
          v-model="prefinal"
          class="w-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition py-1 px-2 rounded-md border border-gray-300"
        />
      </div>
      <div>
        <label for="finals" class="font-semibold text-gray-700"
          >Finals (40%)</label
        >
        <input
          id="finals"
          step="0.01"
          type="number"
          placeholder="0.00"
          v-model="finals"
          class="w-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition py-1 px-2 rounded-md border border-gray-300"
        />
      </div>
    </div>
    <p v-if="message" class="text-red-500 mt-4">{{ message }}</p>
    <div class="mt-4 flex items-center gap-2">
      <button
        @click="clearAll"
        class="px-4 py-2 bg-red-400 hover:bg-red-500 text-white rounded-md font-semibold cursor-pointer"
      >
        Clear all
      </button>
      <button
        @click="calculate"
        class="px-4 py-2 bg-blue-400 hover:bg-blue-500 text-white rounded-md font-semibold cursor-pointer"
      >
        Calculate
      </button>
    </div>
    <div
      v-if="gwa"
      class="mt-4 border-2 py-2 px-4 rounded-md"
      :class="resultColor"
    >
      <h1
        class="font-semibold text-md md:text-lg lg:text-xl"
        :class="textColor"
      >
        Your Results
      </h1>
      <div class="mt-2 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <div class="w-full">
          <p class="font-semibold" :class="textColor">Average Grade:</p>
          <span
            class="font-bold text-lg md:text-xl lg:text-2xl"
            :class="textColor"
          >
            {{ gwa }}%
          </span>
        </div>
        <div class="w-full">
          <p class="font-semibold" :class="textColor">GWA:</p>
          <span
            class="font-bold text-lg md:text-xl lg:text-2xl"
            :class="textColor"
          >
            {{ gradeInfo.grade }}
          </span>
        </div>
        <div class="w-full">
          <p class="font-semibold" :class="textColor">Remark:</p>
          <span
            class="font-bold text-lg md:text-xl lg:text-2xl"
            :class="textColor"
          >
            {{ gradeInfo.desc }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
const prelims = ref("");
const midterms = ref("");
const prefinal = ref("");
const finals = ref("");
const message = ref("");
const gwa = ref(0);
const gradeScale = [
  { min: 97.5, max: 100.0, grade: "1.00", desc: "Excellent" },
  { min: 94.5, max: 97.49, grade: "1.25", desc: "Very Good" },
  { min: 91.5, max: 94.49, grade: "1.50", desc: "Very Good" },
  { min: 86.5, max: 91.49, grade: "1.75", desc: "Very Good" },
  { min: 81.5, max: 86.49, grade: "2.00", desc: "Satisfactory" },
  { min: 76.0, max: 81.49, grade: "2.25", desc: "Satisfactory" },
  { min: 70.5, max: 75.99, grade: "2.50", desc: "Satisfactory" },
  { min: 65.0, max: 70.49, grade: "2.75", desc: "Fair" },
  { min: 59.5, max: 64.99, grade: "3.00", desc: "Fair" },
  { min: 0, max: 59.49, grade: "5.00", desc: "Failed" },
];
const calculate = () => {
  if (!prelims.value || !midterms.value || !prefinal.value || !finals.value) {
    message.value = "Please fill in all fields.";
    return;
  }
  const grades = [prelims.value, midterms.value, prefinal.value, finals.value];
  const weights = [0.2, 0.2, 0.2, 0.4];
  const total = grades.reduce(
    (total, val, i) => total + (parseFloat(val) || 0) * weights[i],
    0
  );
  gwa.value = total.toFixed(2);
  message.value = "";
};
const clearAll = () => {
  prelims.value = "";
  midterms.value = "";
  prefinal.value = "";
  finals.value = "";
  message.value = "";
  gwa.value = 0;
};
const gradeInfo = computed(() => {
  if (gwa.value === null || gwa.value < 0 || gwa.value > 100) {
    return { grade: "N/A", desc: "Invalid score" };
  }
  return gradeScale.find(
    (range) => gwa.value >= range.min && gwa.value <= range.max
  );
});
const resultColor = computed(() => {
  if (!gradeInfo.value) return "bg-gray-100 border-gray-300";
  if (gradeInfo.value.desc === "Excellent")
    return "bg-green-100 border-green-300";
  if (gradeInfo.value.desc === "Very Good")
    return "bg-blue-100 border-blue-300";
  if (gradeInfo.value.desc === "Satisfactory")
    return "bg-yellow-100 border-yellow-300";
  if (gradeInfo.value.desc === "Fair") return "bg-orange-100 border-orange-300";
  if (gradeInfo.value.desc === "Failed") return "bg-red-100 border-red-300";
  return "bg-gray-100 border-gray-300";
});
const textColor = computed(() => {
  if (!gradeInfo.value) return "text-gray-700";
  if (gradeInfo.value.desc === "Excellent") return "text-green-700";
  if (gradeInfo.value.desc === "Very Good") return "text-blue-700";
  if (gradeInfo.value.desc === "Satisfactory") return "text-yellow-700";
  if (gradeInfo.value.desc === "Fair") return "text-orange-700";
  if (gradeInfo.value.desc === "Failed") return "text-red-700";
  return "text-gray-700";
});
</script>
