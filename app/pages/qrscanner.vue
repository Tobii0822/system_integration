<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue'

const scannedText = ref('')
let html5QrCode: any = null

onMounted(async () => {
  if (process.client) {
    const { Html5Qrcode } = await import('html5-qrcode')

    html5QrCode = new Html5Qrcode('reader')

    try {
      await html5QrCode.start(
        { facingMode: 'environment' }, // back camera
        {
          fps: 10,
          qrbox: { width: 250, height: 250 }
        },
        (decodedText: string) => {
          scannedText.value = decodedText
          console.log('QR Code:', decodedText)
        },
        (errorMessage: string) => {
          // ignore scan errors
        }
      )
    } catch (err) {
      console.error(err)
      alert('Camera access failed')
    }
  }
})

onBeforeUnmount(async () => {
  if (html5QrCode) {
    try {
      await html5QrCode.stop()
      await html5QrCode.clear()
    } catch (err) {
      console.error(err)
    }
  }
})
</script>

<template>
  <div class="container">
    <h1>Nuxt QR Scanner</h1>

    <div id="reader"></div>

    <div v-if="scannedText" class="result">
      <h3>Scanned Result:</h3>
      <p>{{ scannedText }}</p>

      <a
        v-if="scannedText.startsWith('http')"
        :href="scannedText"
        target="_blank"
      >
        Open Link
      </a>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;
  text-align: center;
  font-family: Arial, sans-serif;
}

#reader {
  width: 100%;
  margin-top: 20px;
}

.result {
  margin-top: 20px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 10px;
  word-break: break-word;
}
</style>