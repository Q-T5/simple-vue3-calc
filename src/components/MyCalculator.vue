<template>
  <div class="w-80 grid grid-cols-3 space-x-3 space-y-3 border-2 rounded-md border-gray-500 px-2 py-4">
    <div class="daisyui-btn daisyui-btn-sm no-animation" @click="showAdvanced ? showAdvanced = false : showAdvanced = true, disableOperators ? disableOperators = false : disableOperators = false, showAdvancedButtonAlreadyClicked == 0 ? showAdvancedButtonAlreadyClicked = 1 : showAdvancedButtonAlreadyClicked = 0, $emit('changeNotifMessage', showAdvancedButtonAlreadyClicked)">
      Advanced
    </div>
    <div class="bg-gray-300 col-span-3 py-2 text-2xl rounded-md text-end text-black px-2" v-text="query"></div>
    <button class="calc-buttons" @click="number(0)">0</button>
    <button class="calc-buttons" @click="number(1)">1</button>
    <button class="calc-buttons" @click="number(2)">2</button>
    <button class="calc-buttons" @click="number(3)">3</button>
    <button class="calc-buttons" @click="number(4)">4</button>
    <button class="calc-buttons" @click="number(5)">5</button>
    <button class="calc-buttons" @click="number(6)">6</button>
    <button class="calc-buttons" @click="number(7)">7</button>
    <button class="calc-buttons" @click="number(8)">8</button>
    <button class="calc-buttons" @click="number(9)">9</button>
    <button class="calc-buttons" @click="calculate(), $emit('addToHistory', historyBuffer)">=</button>
    <button class="calc-buttons" @click="sign('+')" :disabled="disableOperators">+</button>
    <button class="calc-buttons" @click="sign('-')" :disabled="disableOperators">-</button>
    <button class="calc-buttons" @click="sign('/')" :disabled="disableOperators">/</button>
    <button class="calc-buttons" @click="sign('*')" :disabled="disableOperators">*</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators ? disableOperators = false : disableOperators = true, $emit('advMathBtnClicked')">SIN</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators ? disableOperators = false : disableOperators = true, $emit('advMathBtnClicked')">COS</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators ? disableOperators = false : disableOperators = true, $emit('advMathBtnClicked')">TAN</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators ? disableOperators = false : disableOperators = true, $emit('advMathBtnClicked')">SQRT</button>
    <button class="calc-buttons col-span-2" v-show="showAdvanced" @click="disableOperators ? disableOperators = false : disableOperators = true, $emit('advMathBtnClicked')">CBRT</button>
    <button class="calc-buttons col-span-3" @click="clearQuery">C</button>
  </div>
</template>

<script>
  export default {
    "name": "MyCalculator",
    "emits": ["addToHistory", "changeNotifMessage", "advMathBtnClicked"],
    "props": ["historyCalc"],
    data() {
      return {
        "query": "0",
        "previous": "",
        "operator": "",
        "result": "",
        "historyBuffer": [],
        "showAdvanced": false,
        "disableOperators": false,
        "showAdvancedButtonAlreadyClicked": 0
      }
    },
    "methods": {
      number(num) {
        if(this.query == 0) {
          this.query = "";
        }

        this.query += num;
      },

      clearQuery() {
        this.query = "0"; 
      },

      sign(operator) {
        this.previous = this.query;
        this.operator = operator;
        this.query = "0";
      },

      calculate() {
        const calculation = {
          "query": this.query,
          "previous": this.previous,
          "operator": this.operator
        }

        this.historyBuffer.push(calculation);

        if(this.operator == "+") {
          this.add(this.query, this.previous);
        } else if(this.operator == "-") {
          this.minus(this.query, this.previous);
        } else if(this.operator == "*") {
          this.product(this.query, this.previous);
        } else if(this.operator == "/") {
          this.divide(this.query, this.previous)
        }
      },

      add(num1, num2) {
        this.query = (Number(num1) + Number(num2)).toString();
      },

      minus(num1, num2) {
        this.query = (Number(num2) - Number(num1)).toString();
      },

      product(num1, num2) {
        this.query = (Number(num1) * Number(num2)).toString();
      },

      divide(num1, num2) {
        this.query = (Number(num2) / Number(num1)).toString();
      }
    },
    "watch": {
      historyCalc() {
        if(this.historyCalc != null) {
          this.query = this.historyBuffer[this.historyCalc].query;
          this.previous = this.historyBuffer[this.historyCalc].previous;
          this.operator = this.historyBuffer[this.historyCalc].operator;
        }

        this.calculate();
      }
    }
  }
</script>