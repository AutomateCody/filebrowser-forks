<!-- src/components/Battery.vue -->
<template>
    <div class="battery">
      <i class="material-icons">battery_std</i>
      <span>{{ batteryLevel }}%</span>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue';
  
  const batteryLevel = ref(0);
  
  const updateBatteryLevel = () => {
    if (typeof fully !== 'undefined' && fully.getBatteryLevel) {
      batteryLevel.value = fully.getBatteryLevel();
    }
  };
  
  onMounted(() => {
    updateBatteryLevel();
    setInterval(updateBatteryLevel, 60000); // Update every minute
  });
  </script>
  
  <style scoped>
  .battery {
    display: flex;
    align-items: center;
  }
  .battery .material-icons {
    margin-right: 5px;
  }
  </style>