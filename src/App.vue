<template>
  <div class="shadow-lg py-2 text-center text-2xl underline">
    Simple Calculator with History
  </div>

  <div class="flex flex-col items-center md:flex-row md:justify-center md:items-start md:space-x-3 pt-2">
    <my-calculator @addToHistory="updateHistory" @changeNotifMessage="showNotificationPopup" :historyCalc="previousCalc" @advMathBtnClicked="notifAdvMthBtnClicked" />
    <div class="pt-2 md:pt-0">
      <h3 class="text-2xl underline pb-2 uppercase text-center">History</h3>
      <div v-for="(calc, index) in pastCalculations" :key="calc.previous" class="">
        <div class="border-2 text-2xl mt-2 rounded-lg border-black w-32 text-center hover:border-green-400" @click="previousCalc = index,notifMessage = 'Showing Result for Clicked Calculation.', showNotification = true">
          <p>
            {{ calc.previous }}
            {{ calc.operator }}
            {{ calc.query }}
          </p>
        </div>
      </div>
    </div>
  </div>

  <div class="relative">
    <notification class="fixed md:left-[35%] bottom-2 left-2 daisyui-alert max-w-fit rounded-md border-l-8" v-show="showNotification" :message="notifMessage" />
  </div>
</template>

<script>
import Notification from './components/Notification.vue';
import MyCalculator from './components/MyCalculator.vue'
  export default {
    "name": "App",
    "components": {
      MyCalculator, Notification
    },
    data() {
      return {
        "pastCalculations": [],
        "previousCalc": null,
        "showNotification": false,
        "notifMessage": "",
        "anyAdvMthBtnClicked": 0
      }
    },
    methods: {
      updateHistory(historyBuffer) {
        this.pastCalculations = historyBuffer;
      }, 
      showNotificationPopup(showAdvancedButtonAlreadyClicked) {
        if(showAdvancedButtonAlreadyClicked == 1) {
          this.notifMessage = "Showing Advanced View. Click Again to Close."
          this.showNotification = true;
        }

      },
      notifAdvMthBtnClicked() {
        if(this.anyAdvMthBtnClicked == 0) {
          this.anyAdvMthBtnClicked = 1;
          this.notifMessage = "Disabled Operators(+, /, *, -). Click Again to Enable."
          this.showNotification = true;
        } else {
          this.anyAdvMthBtnClicked = 0;
        }
      }
    },
    "watch": {
      showNotification() {
        if(this.showNotification == true) {
          setTimeout(() => {
            this.showNotification = false;
          }, 2000);
        }
      }
    }
  }
</script>