<template>
  <main class="wrapper">
    <div id="letter" class="a6" ref="letter">
      <div class="line-1">{{ letterState.text }}</div>
      <div class="line-2">1000 Hilltop Circle</div>
      <div class="line-3">Baltimore, MD</div>
      <div class="line-4">20852</div>
    </div>
    <p>{{ letterState }}</p>
    <input type="text" v-model="letterState.text" />
  </main>
</template>

<script>
import { reactive, ref, onMounted, watchEffect } from 'vue'

export default {
  setup() {
    const letter = ref(null)

    const letterState = reactive({
      offsetLeft: 0,
      width: 0,
      text: ''
    })

    watchEffect(() => {
      if (letterState.text) {
        letterState.offsetLeft = letter.value.offsetLeft
      }
    })

    onMounted(() => {
      letterState.width = letter.value.offsetWidth
      console.log({ window })
    })

    return {
      letter,
      letterState
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
</style>
