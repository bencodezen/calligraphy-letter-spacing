<template>
  <main class="wrapper">
    <div id="letter" class="a6" ref="envelope">
      <div
        class="metric-line"
        :style="`width: ${lineState.width}px; top: ${metricState.top}px;`"
      >
        {{ lineState.widthInInches }} in
      </div>
      <div class="middle-line"></div>
      <div class="line-1" @click="setDimensions">
        {{ envelopeState.text.line1 }}
      </div>
      <div class="line-2" @click="setDimensions">
        {{ envelopeState.text.line2 }}
      </div>
      <div class="line-3" @click="setDimensions">
        {{ envelopeState.text.line3 }}
      </div>
      <div class="line-4" @click="setDimensions">
        {{ envelopeState.text.line4 }}
      </div>
    </div>
    <form @submit.prevent>
      <input type="text" v-model="envelopeState.text.line1" />
      <input type="text" v-model="envelopeState.text.line2" />
      <input type="text" v-model="envelopeState.text.line3" />
      <input type="text" v-model="envelopeState.text.line4" />
    </form>
  </main>
</template>

<script>
import { computed, reactive, ref, onMounted } from 'vue'

export default {
  setup() {
    const envelope = ref(null)

    const envelopeState = reactive({
      offsetLeft: 0,
      offsetTop: 0,
      width: 0,
      text: {
        line1: '',
        line2: '',
        line3: '',
        line4: ''
      }
    })

    const lineState = reactive({
      width: 0,
      widthInInches: computed(() => {
        return (6.5 * lineState.ratio).toFixed(2)
      }),
      top: 0,
      height: 0,
      ratio: computed(() => {
        return lineState.width / envelopeState.width
      })
    })

    const metricState = reactive({
      top: computed(() => {
        return lineState.top
      })
    })

    const setDimensions = event => {
      lineState.width = event.srcElement.offsetLeft
      lineState.top = event.srcElement.offsetTop
      lineState.height = event.srcElement.offsetHeight
    }

    onMounted(() => {
      envelopeState.width = envelope.value.offsetWidth
      envelopeState.offsetLeft = envelope.value.offsetLeft
      envelopeState.offsetTop = envelope.value.offsetTop
    })

    return {
      envelope,
      envelopeState,
      lineState,
      metricState,
      setDimensions
    }
  }
}
</script>

<style>
.wrapper {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#letter {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px solid black;
  position: relative;
}

.metric-line {
  width: 50px;
  border-bottom: 3px solid #ddd;
  text-align: center;
  position: absolute;
  left: 0;
}

.middle-line {
  border-left: 5px dashed #eee;
  height: calc(4.75in - 0.1in);
  position: absolute;
  z-index: -1;
}

.line-1,
.line-3,
.line-4 {
  display: inline-block;
  font-family: 'Dancing Script', cursive;
  font-weight: 800;
}

.line-1 {
  font-size: 3.75rem;
}

.line-2 {
  display: inline-block;
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 1.75rem;
  margin-top: -5px;
  text-transform: uppercase;
}

.line-3,
.line-4 {
  font-size: 2rem;
}

.line-4 {
  letter-spacing: 10px;
}

.test {
  width: 2in;
  height: 1in;
}

.a6 {
  height: 4.75in;
  width: 6.5in;
}

form {
  display: flex;
  flex-direction: column;
  margin-top: 50px;
}

input {
  font-size: 1.5rem;
  margin-bottom: 10px;
}
</style>
