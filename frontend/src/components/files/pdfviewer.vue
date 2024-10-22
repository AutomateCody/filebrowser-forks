<template></template>

<script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import { defineEmits } from 'vue';

  const emit = defineEmits(['close']);

  const props = withDefaults(
  defineProps<{
    url: string;
  }>(),
  {
  }
);

  const url = props.url;

  
  const openPDF = () => {
    if (typeof fully !== 'undefined') {
        console.log(url)
        fully.showPdf(url)
    }
    else {
        console.log(url)
        console.log("Fully not found!")
        alert("Fully not found!")
    }
  };
  
  onMounted(() => {
    console.log('Component mounted');
    openPDF();
    emit('close');
  });
  </script>
``