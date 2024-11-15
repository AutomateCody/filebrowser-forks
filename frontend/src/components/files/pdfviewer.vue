<template>

<div id="pdf-wrapper" style="width: 100%; height: 100%;" @touchstart="handleTouchStart" @touchmove="handleTouchMove">
  <vue-pdf-app :pdf="url" :config="pdfConfig" theme="dark" @open="openHandler" @pages-rendered="pagesRenderedHandler">
    <template #toolbar-left-prepend>
      <action icon="close" @action="emit('close')" />
    </template>
  </vue-pdf-app>
</div>



</template>

<script setup lang="ts">
  import { ref, onMounted, watch, onUnmounted } from 'vue';
  import { defineEmits } from 'vue';
  import Action from "@/components/header/Action.vue";
  import VuePdfApp from "vue3-pdf-app";
  import "vue3-pdf-app/dist/icons/main.css";

  const emit = defineEmits(['close']);


  const props = withDefaults(
  defineProps<{
    url: string;
  }>(),
  {
  }
);

  const url = props.url;


  const info = ref([]);

  const openHandler = async (pdfApp: any) => {
  console.log("openHandler");
  info.value = [];
  const metadata = await pdfApp.pdfDocument.getMetadata().catch(console.error.bind(console));

  if (!metadata) return;
  const props = Object.keys(metadata.info);
  props.forEach((prop) => {
    const obj = {
      name: prop,
      value: metadata.info[prop]
    };
    //info.value.push(obj);
    console.log(obj);
  });
};


  const pdfConfig = ref({
  sidebar: false,
  secondaryToolbar: {
    cursorSelectTool: false,
    cursorHandTool: false,
    documentProperties: false,
  },
  toolbar: {
    toolbarViewerRight: false,
  }
})

  
//  const openPDF = () => {
//    if (typeof fully !== 'undefined') {
//        console.log(url)
//        fully.showPdf(url)
//    }
//    else {
//        console.log(url)
//        console.log("Fully not found!")
//        alert("Fully not found!")
//    }
//  };

let initialDistance = 0;
let pdfAppInstance: any = null;

const pagesRenderedHandler = (pdfApp: any) => {
  pdfAppInstance = pdfApp;
};

const handleTouchStart = (event: TouchEvent) => {
  if (event.touches.length === 2) {
    const touch1 = event.touches[0];
    const touch2 = event.touches[1];
    initialDistance = Math.hypot(touch2.pageX - touch1.pageX, touch2.pageY - touch1.pageY);
  }
};

const handleTouchMove = (event: TouchEvent) => {
  if (event.touches.length === 2 && initialDistance > 0) {
    const touch1 = event.touches[0];
    const touch2 = event.touches[1];
    const currentDistance = Math.hypot(touch2.pageX - touch1.pageX, touch2.pageY - touch1.pageY);
    const scaleChange = currentDistance / initialDistance;

    if (pdfAppInstance) {
      pdfAppInstance.pdfViewer.currentScaleValue *= scaleChange;
      initialDistance = currentDistance; // Update the initial distance for the next move event
    }
  }
};
  
  </script>

<style>
.pdf-app.dark {
  --sidebar-width: 0;
  --pdf-app-background-color: var(--background);
  --pdf-toolbar-color: var(--surfacePrimary);
}

.verticalToolbarSeparator {
  display: none;
}

.vue-pdf-app-icon::before,
.vue-pdf-app-icon::after {
  font-size: 1.5rem !important;
}


.pdf-app #toolbarContainer {
  height: 3em !important;
}

.pdf-app #toolbarContainer {
  padding: 2px !important;
}

.pdf-app .toolbarButton {
  width: 43px !important;
  height: 3em !important;
}

.pdf-app .toolbarLabel {
  margin-top: 10px !important;
}

.pdf-app .splitToolbarButtonSeparator {
  padding: 13px 0 !important;
}

.pdf-app .toolbarField.pageNumber {
  height: 3em !important;
}

.pdf-app[class] .dropdownToolbarButton > select {
  height: 3em !important;
}

.pdf-app .dropdownToolbarButton::after {
  top: 12px !important;
}

.pdf-app[class] .verticalToolbarSeparator {
  height: 27px !important
}

</style>
``