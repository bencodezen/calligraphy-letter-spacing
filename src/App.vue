<template>
  <main class="wrapper">
    <div id="letter" class="test" ref="letter">
      <div class="line-1">{{ letterState.text }}</div>
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
      width: 0
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
  border: 2px solid black;
  text-align: center;
}

.line-1 {
  display: inline-block;
}

.test {
  width: 2in;
  height: 1in;
}

.a6 {
  width: 4.75in;
  height: 6.5in;
}
</style>
