<script setup lang="ts">
import BlurredCard from './ui/BlurredCard.vue';
import { onMounted, onUnmounted, ref } from 'vue';
import { BarcodeScannerOptimized } from 'choco-barcode-scanner';

const emit = defineEmits<{ (e: 'close'): void; (e: 'success', barcode: string[]): void; }>();

const barcode = ref<string[]>([])
const isCameraAllowed = ref<boolean>(true);
const drawSymbols = ref<boolean>(true);

const close = () => {
  emit('close');
};

const onSuccess = (barcodes: string[]) => {
  barcode.value = barcodes;
  emit('success', barcodes);
};


const initBarcodeScanner = () => {
  BarcodeScannerOptimized.init({
    container: 'barcode-scanner',
    drawSymbols: drawSymbols.value,
    settings: {
      width: window.innerWidth,
      height: window.innerHeight,
    },
    onSuccess
  }).catch((error) => {
    if (error.message === "NOT_ALLOWED") {
      isCameraAllowed.value = false;
    }
  });
};

const restartScanner = () => {
  BarcodeScannerOptimized.destroy();
  initBarcodeScanner();
};

const toggleDrawSymbols = () => {
  drawSymbols.value = !drawSymbols.value;
  restartScanner();
};

onMounted(() => initBarcodeScanner());
onUnmounted(() => BarcodeScannerOptimized.destroy());
</script>

<template>
  <div class="barcode">
    <BlurredCard class="barcode--appear fixed top-4 left-4 right-4 px-4 py-3 flex items-center z-50 rounded-xl">
      <button class="px-3 py-2 bg-white text-black rounded-lg flex items-center gap-2" @click="toggleDrawSymbols">
        <span>Outline {{ drawSymbols ? 'active' : 'inactive' }}</span>

        <span :class="{
          'text-green-700': drawSymbols,
        }">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
            <path fill="currentColor"
              d="M7 21v-2h2v2zm4 0v-2h2v2zm4 0v-2h2v2zm4 0v-2h2v2zm0-4v-2h2v2zm0-4v-2h2v2zm0-4V7h2v2zM3 21V3h18v2H5v16z" />
          </svg>
        </span>
      </button>

      <div v-if="!isCameraAllowed" class="ml-auto text-white">Camera access denied</div>

      <button class="ml-auto p-2 bg-gray-500 text-white rounded-lg" @click="close">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
          <path fill="currentColor"
            d="m12 13.4l-4.9 4.9q-.275.275-.7.275t-.7-.275t-.275-.7t.275-.7l4.9-4.9l-4.9-4.9q-.275-.275-.275-.7t.275-.7t.7-.275t.7.275l4.9 4.9l4.9-4.9q.275-.275.7-.275t.7.275t.275.7t-.275.7L13.4 12l4.9 4.9q.275.275.275.7t-.275.7t-.7.275t-.7-.275z" />
        </svg>
      </button>
    </BlurredCard>

    <div class="barcode__area">
      <div id="barcode-scanner"></div>
    </div>

    <BlurredCard v-if="barcode.length" class="barcode--appear fixed bottom-4 left-4 right-4 px-4 py-3 text-white rounded-xl">
      <p>Scanned barcodes: {{ barcode.join(', ') }}</p>
    </BlurredCard>
  </div>
</template>

<style>
.barcode--appear{
  opacity: 0;
  animation: appear-from-top 0.3s ease forwards 1s
}

.barcode__area {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #000;
}

#barcode-scanner {
  position: relative;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

#barcode-scanner canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
}

#barcode-scanner video {
  position: absolute;
  display: block;
  max-width: 100%;
  height: 100%;
  object-fit: cover;
}

@keyframes appear-from-top {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}
</style>