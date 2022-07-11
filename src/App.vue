<template>
  <div class="shadow-lg py-2 text-center text-2xl underline">
    Simple Calculator
  </div>

  <div class="flex flex-col items-center md:flex-row md:justify-center md:items-start md:space-x-3 pt-2">
    <!-- this is the calculator itself -->
    <my-calculator @addToHistory="updateHistory" @changeNotifMessage="showNotificationPopup" :historyCalc="previousCalc" @advMathBtnClicked="notifAdvMthBtnClicked" :dumpHistory="clearHistory" @showHistoryTab="showTheHistoryTab = !showTheHistoryTab" />
  </div>

  <!-- this is the history section -->
  <div class="p-3 fixed w-2/3 top-[49px] h-screen flex flex-col items-start backdrop-blur-md bg-gray-500/75 overflow-auto" v-show="showTheHistoryTab">
    <h3 class="text-2xl underline pb-2 uppercase inline-block">History
      <button class="daisyui-btn daisyui-btn-xs ml-2 no-animation" @click="authClearHistory">CLEAR</button>
      <button class="daisyui-btn daisyui-btn-xs daisyui-btn-ghost ml-1 no-animation" @click="showTheHistoryTab = false">CLOSE</button>
    </h3>
    <div v-for="(calc, index) in pastCalculations" :key="calc.previous">
      <div class="border-2 px-2 text-xl mt-2 rounded-sm border-gray-400 text-center hover:border-gray-500" @click="previousCalc = index, 
      notifMessage = 'Showing Result for Clicked Calculation.', showNotification = true">
        <p> {{ calc.previous }} {{ calc.operator }} {{ calc.current }} </p>
      </div>
    </div>
  </div>

  <!-- this is the notification popup -->
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
        "clearHistory": false,
        "showTheHistoryTab": false
      }
    },
    methods: {
      updateHistory(historyBuffer) {
        this.pastCalculations = historyBuffer;
      }, 
      showNotificationPopup(displayAdvanced) {
        if(displayAdvanced === true) {
          this.notifMessage = "Showing Advanced View. Click Again to Close."
          this.showNotification = true;
        }

      },
      notifAdvMthBtnClicked(disableOperators) {
        if(disableOperators === true) {
          this.notifMessage = "Disabled Operators(+, /, *, -). Click 'C' to Enable."
          this.showNotification = true;
        }
      },
      authClearHistory() {
        if(this.pastCalculations.length > 0) {
          this.clearHistory = true;

          this.notifMessage = "Cleared History."
          this.showNotification = true;

          setTimeout(() => {
            this.clearHistory = false;
          }, 2000);
        } else {
          this.notifMessage = "Nothing to Clear."
          this.showNotification = true;
        }
      }
    },
    "watch": {
      showNotification() {
        if(this.showNotification === true) {
          setTimeout(() => {
            this.showNotification = false;
          }, 2000);
        }
      }
    }
  }
</script>