<template>
  <div class="p-10 flex flex-col items-center justify-center md:flex-row">
    <div class="w-80 grid grid-cols-3 space-x-3 space-y-3 border-2 rounded-md border-gray-500 px-4 py-4">
      <div class="bg-slate-200 col-span-3 py-2 text-2xl rounded-md text-end" v-text="query"></div>
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
      <button class="calc-buttons" @click="calculate">=</button>
      <button class="calc-buttons" @click="sign('+')">+</button>
      <button class="calc-buttons" @click="sign('-')">-</button>
      <button class="calc-buttons" @click="sign('/')">/</button>
      <button class="calc-buttons" @click="sign('*')">*</button>
      <button class="calc-buttons col-span-3" @click="clearQuery()">C</button>
    </div>

    <div>
      Hello World B
    </div>
  </div>
</template>

<script>
  export default {
    "name": "MyCalculator",
    data() {
      return {
        "query": "0",
        "previous": "",
        "operator": "",
        "result": ""
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

        console.log(this.previous, this.operator, this.querry);
      },

      calculate() {
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
        this.query = Number(num1) + Number(num2);
      },

      minus(num1, num2) {
        this.query = Number(num2) - Number(num1);
      },

      product(num1, num2) {
        this.query = Number(num1) * Number(num2);
      },

      divide(num1, num2) {
        this.query = Number(num2) / Number(num1);
      }
    }
  }
</script>