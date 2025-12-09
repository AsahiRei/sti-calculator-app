<template>
  <div class="mt-4 bg-white border border-gray-300 rounded-md p-4">
    <h1 class="font-semibold text-md md:text-lg lg:text-xl text-gray-700">
      Grade Scale
    </h1>
    <div class="overflow-x-auto mt-4">
      <table class="min-w-full bg-white border-t border-gray-300">
        <thead class="bg-gray-100">
          <tr>
            <th
              class="py-2 px-4 border-b border-gray-300 text-left text-gray-700"
            >
              Average
            </th>
            <th
              class="py-2 px-4 border-b border-gray-300 text-left text-gray-700"
            >
              GWA
            </th>
            <th
              class="py-2 px-4 border-b border-gray-300 text-left text-gray-700"
            >
              Remarks
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in gradeScale" :key="item.grade">
            <td class="py-2 px-4 border-b border-gray-300">{{ item.min }} - {{ item.max }}</td>
            <td class="py-2 px-4 border-b border-gray-300">{{ item.grade }}</td>
            <td class="py-2 px-4 border-b border-gray-300">
              <span
                :class="badgeColor(item.desc)"
                class="px-2 py-1 rounded-md text-white text-sm font-semibold"
              >
                {{ item.desc }}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script setup>
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
const badgeColor = (desc) => {
  switch (desc) {
    case "Excellent":
      return "bg-green-500";
    case "Very Good":
      return "bg-blue-500";
    case "Satisfactory":
      return "bg-yellow-500";
    case "Fair":
      return "bg-orange-500";
    case "Failed":
      return "bg-red-500";
    default:
      return "bg-gray-500";
  }
};
</script>