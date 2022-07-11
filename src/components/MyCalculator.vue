<template>
  <div class="w-80 grid grid-cols-3 space-x-3 space-y-3 border-2 rounded-md border-gray-500 px-2 py-4">
    <div class="daisyui-btn-group col-span-2">
      <div class="daisyui-btn daisyui-btn-sm no-animation" @click="showAdvanced = !showAdvanced, 
      disableOperators = false, displayAdvanced = !displayAdvanced, $emit('changeNotifMessage', displayAdvanced)">
        Advanced
      </div>
      <div class="daisyui-btn daisyui-btn-sm daisyui-btn-ghost no-animation" @click="$emit('showHistoryTab')">History</div>
    </div>
    <div class="bg-gray-300 col-span-3 py-2 text-2xl rounded-md text-end text-black px-2" v-text="current"></div>
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
    <button class="calc-buttons" @click="calculate(), $emit('addToHistory', historyBuffer), 
    disableOperators == true ? disableOperators = false : ''">=</button>
    <button class="calc-buttons" @click="sign('+')" :disabled="disableOperators">+</button>
    <button class="calc-buttons" @click="sign('-')" :disabled="disableOperators">-</button>
    <button class="calc-buttons" @click="sign('/')" :disabled="disableOperators">/</button>
    <button class="calc-buttons" @click="sign('*')" :disabled="disableOperators">*</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators = !disableOperators, $emit('advMathBtnClicked', disableOperators), 
    evalAdvanced('sin')" :disabled="disableOperators">SIN</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators = !disableOperators, $emit('advMathBtnClicked', disableOperators), 
    evalAdvanced('cos')" :disabled="disableOperators">COS</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators = !disableOperators, $emit('advMathBtnClicked', disableOperators), 
    evalAdvanced('tan')" :disabled="disableOperators">TAN</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators = !disableOperators, $emit('advMathBtnClicked', disableOperators), 
    evalAdvanced('sqrt')" :disabled="disableOperators">SQRT</button>
    <button class="calc-buttons" v-show="showAdvanced" @click="disableOperators = !disableOperators, $emit('advMathBtnClicked', disableOperators), 
    evalAdvanced('cbrt')" :disabled="disableOperators">CBRT</button>
    <button class="calc-buttons" @click="clearcurrent(), disableOperators == true ? disableOperators = false : ''" 
    :class="showAdvanced == false ? 'col-span-3' : ''">C</button>
  </div>
</template>

<script>
  export default {
    "name": "MyCalculator",
    "emits": ["addToHistory", "changeNotifMessage", "advMathBtnClicked", "showHistoryTab"],
    "props": ["historyCalc", "dumpHistory"],
    data() {
      return {
        "current": "0",
        "previous": "",
        "operator": "",
        "result": "",
        "historyBuffer": [],
        "showAdvanced": false,
        "disableOperators": false,
        "displayAdvanced": false
      }
    },
    "methods": {
      number(num) {
        if(this.current.startsWith("sin") || this.current.startsWith("cos") || this.current.startsWith("tan") 
        || this.current.startsWith("sqrt") || this.current.startsWith("cbrt")) {
          this.previous += num;
        }

        if(this.current == 0) {
          this.current = "";
        }

          // this.previous = "";
        this.current += num;
      },

      clearcurrent() {
        if(this.current.startsWith("sin") || this.current.startsWith("cos") || this.current.startsWith("tan") 
        || this.current.startsWith("sqrt") || this.current.startsWith("cbrt")) {
          this.current = "0";
          this.previous = "";
        }

        this.current = "0";
      },

      sign(operator) {
        this.previous = this.current;
        this.operator = operator;
        this.current = "0";
      },

      calculate() {
        if(this.current != "" && this.previous != "" && this.operator != "") {
          const calculation = {
          "current": this.current,
          "previous": this.previous,
          "operator": this.operator
          }

          this.historyBuffer.push(calculation);
        } else {
          if(this.current.startsWith("sin")) {
            this.doSin(this.previous);
          } else if(this.current.startsWith("cos")) {
            this.doCos(this.previous);
          } else if(this.current.startsWith("tan")) {
            this.doTan(this.previous);
          } else if(this.current.startsWith("sqrt")) {
            this.doSqrt(this.previous)
          } else if(this.current.startsWith("cbrt")) {
            this.doCbrt(this.previous);
          }
        }

        if(this.operator == "+") {
          this.add(this.current, this.previous);
        } else if(this.operator == "-") {
          this.minus(this.current, this.previous);
        } else if(this.operator == "*") {
          this.product(this.current, this.previous);
        } else if(this.operator == "/") {
          this.divide(this.current, this.previous)
        }
      },

      add(num1, num2) {
        this.current = (Number(num1) + Number(num2)).toString();
        this.operator = "";
        this.previous = "";
      },

      minus(num1, num2) {
        this.current = (Number(num2) - Number(num1)).toString();
        this.operator = "";
        this.previous = "";
      },

      product(num1, num2) {
        this.current = (Number(num1) * Number(num2)).toString();
        this.operator = "";
        this.previous = "";
      },

      divide(num1, num2) {
        this.current = (Number(num2) / Number(num1)).toString();
        this.operator = "";
        this.previous = "";
      },

      evalAdvanced(operation) {
        this.previous = "";
        if(operation == "sin") {
          this.current = "sin ";
        } else if(operation == "cos") {
          this.current = "cos ";
        } else if(operation == "tan") {
          this.current = "tan ";
        } else if(operation == "sqrt") {
          this.current = "sqrt ";
        } else if(operation == "cbrt") {
          this.current = "cbrt ";
        }
      },

      doSin(num) {
        this.current = (Math.sin(+num)).toString();
      },

      doCos(num) {
        this.current = (Math.cos(+num)).toString();
      },

      doTan(num) {
        this.current = (Math.tan(+num)).toString();
      },

      doSqrt(num) {
        this.current = (Math.sqrt(+num)).toString();
      },

      doCbrt(num) {
        this.current = (Math.cbrt(+num)).toString();
      }
    },
    "watch": {
      historyCalc() {
        if(this.historyCalc != null) {
          this.current = this.historyBuffer[this.historyCalc].current;
          this.previous = this.historyBuffer[this.historyCalc].previous;
          this.operator = this.historyBuffer[this.historyCalc].operator;
        }

        this.calculate();
      },

      dumpHistory() {
        if(this.dumpHistory == true) {
          if(this.historyBuffer.length > 0) {
            this.historyBuffer.length = 0;
          }
        }
      }
    }
  }
</script>