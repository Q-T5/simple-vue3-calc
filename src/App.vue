<template>
  <div class="shadow-lg py-4 text-center text-2xl underline">
    Simple Calculator with History
  </div>
  <my-calculator @addToHistory="updateHistory" />
  <h3 class="flex justify-center text-2xl underline pb-2 uppercase">History</h3>
  <div v-for="calc in pastCalculations" :key="calc" class="flex justify-center space-y-5">
      <div class="border-2 text-2xl mt-2 rounded-lg border-black w-32 text-center hover:border-green-400">
        <p>
          {{ calc.previous }}
          {{ calc.operator }}
          {{ calc.query }}
        </p>
      </div>
  </div>
</template>

<script>
import MyCalculator from './components/MyCalculator.vue'
  export default {
    "name": "App",
    "components": {
      MyCalculator
    },
    data() {
      return {
        "pastCalculations": [],
      }
    },
    methods: {
      updateHistory(historyBuffer) {
        console.log("size of AppComp history buffer before: " + this.pastCalculations.length);
        this.pastCalculations = historyBuffer;
        console.log("size of AppComp history buffer after: " + this.pastCalculations.length);
        console.log(this.pastCalculations);
      }
    }
  }
</script>

<style scoped>

</style>