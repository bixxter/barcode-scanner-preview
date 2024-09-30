<script setup lang="ts">
import { ref } from 'vue';
import BarcodeScanner from './components/BarcodeScanner.vue';

const isBarcodeScannerActive = ref<boolean>(false);

const openBarcodeScanner = () => isBarcodeScannerActive.value = true
const closeBarcodeScanner = () => isBarcodeScannerActive.value = false;
</script>

<template>
  <main class="flex flex-col items-center justify-center h-screen">
    <section>
      <h1 class="text-xl text-center flex items-center gap-2">

        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
          <path fill="currentColor"
            d="M1 21v-5h2v3h3v2zm17 0v-2h3v-3h2v5zM4 18V6h2v12zm3 0V6h1v12zm3 0V6h2v12zm3 0V6h3v12zm4 0V6h1v12zm2 0V6h1v12zM1 8V3h5v2H3v3zm20 0V5h-3V3h5v5z" />
        </svg>
        <span>Barcode Scanner Preview</span>
      </h1>

      <button class="w-full py-2 px-3 mt-4 bg-black text-white rounded-lg" @click="openBarcodeScanner">
        Open Barcode Scanner
      </button>

      <a class="w-full py-2 px-3 mt-4 bg-red-500 text-white rounded-lg block text-center"
        href="https://www.npmjs.com/package/choco-barcode-scanner" target="_blank">
        Installation
      </a>
    </section>

    <BarcodeScanner v-if="isBarcodeScannerActive" @close="closeBarcodeScanner" />
  </main>
</template>
