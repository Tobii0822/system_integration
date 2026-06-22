<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

const videoRef = ref<HTMLVideoElement | null>(null)
const scannedValue = ref('')
const scanning = ref(false)

let scanner: any = null

const startScanner = async () => {
  if (!videoRef.value) return

  const { default: QrScanner } = await import('qr-scanner')

  scanner = new QrScanner(
    videoRef.value,
    async (result: any) => {
      scannedValue.value = result.data

      // Stop after successful scan
      await scanner.stop()
      scanning.value = false
    },
    {
      returnDetailedScanResult: true,
      highlightScanRegion: true,
      highlightCodeOutline: true
    }
  )

  await scanner.start()
  scanning.value = true
}

const stopScanner = async () => {
  if (scanner) {
    await scanner.stop()
    scanner.destroy()
    scanner = null
  }

  scanning.value = false
}

onMounted(() => {
  startScanner()
})

onBeforeUnmount(() => {
  stopScanner()
})
</script>

<template>
  <div class="scanner-container">
    <div class="scanner-box">
      <video ref="videoRef"></video>

      <div class="scanner-overlay">
        <div class="scanner-frame"></div>
      </div>
    </div>

    <div class="result">
      <h3>QR Result</h3>

      <p v-if="scannedValue">
        {{ scannedValue }}
      </p>

      <p v-else>
        Waiting for scan...
      </p>
    </div>

    <div class="actions">
      <button
        v-if="!scanning"
        @click="startScanner"
      >
        Start Scanner
      </button>

      <button
        v-if="scanning"
        @click="stopScanner"
      >
        Stop Scanner
      </button>
    </div>
  </div>
</template>

<style scoped>
.scanner-container {
  max-width: 500px;
  margin: auto;
}

.scanner-box {
  position: relative;
  overflow: hidden;
  border-radius: 16px;
}

video {
  width: 100%;
  display: block;
  border-radius: 16px;
}

.scanner-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.scanner-frame {
  width: 220px;
  height: 220px;
  border: 4px solid #22c55e;
  border-radius: 12px;
  box-shadow: 0 0 0 9999px rgba(0, 0, 0, 0.45);
}

.result {
  margin-top: 20px;
}

.actions {
  margin-top: 20px;
}

button {
  background: #22c55e;
  color: white;
  border: none;
  padding: 10px 18px;
  border-radius: 8px;
  cursor: pointer;
}
</style>