<template>
  <main>
    <section class="title">
      <img src="./assets/capybarapture.jpeg" alt="Capybara" class="title-image" />
      <h1>Capybara Testing Environment</h1>
    </section>

    <div v-if="!isAModuleActive()" class="d-flex flex-column gap-2">
      <button class="btn bg-primary" type="button" @click="moduleFlags.gptFlag = !moduleFlags.gptFlag">Gpt Live 1</button>
      <!-- <button class="btn bg-primary" type="button">Stop</button> -->
    </div>

    <div v-if="moduleFlags.gptFlag" class="d-flex flex-column gap-4">
      <button class="btn btn-danger align-self-end" type="button" aria-label="Close module" @click="exitModule()">&times;</button>
      <gpt-live1 />
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import GptLive1 from './components/gptLive1.vue'

export default defineComponent({
  components: {
    GptLive1
  },
  data() {
    return {
      moduleFlags:{
        gptFlag: false
      } as Record<string, boolean>
    }
  },
  methods: {
    isAModuleActive() {
      return Object.keys(this.moduleFlags).some(key => this.moduleFlags[key])
    },
    exitModule() {
      Object.keys(this.moduleFlags).forEach(key => this.moduleFlags[key] = false)
    }
  }
})
</script>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  padding: 3rem 1rem;
}

.title {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 0.75rem;
}

.title h1 {
  font-size: 1.5rem;
}

.title-image {
  width: 160px;
  height: 128px;
}

.controls {
  display: flex;
  gap: 1rem;
}

.btn {
  padding: 0.6rem 1.6rem;
  font-size: 1rem;
  font-weight: bold;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: #fff;
}

</style>
