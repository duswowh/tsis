<template>
  <div id="app">
    <div class="calculator">
      <div class="result" style="grid-area: result">
        {{ eq }}
      </div>

      <button class="funcs" style="grid-area: ac" @click="clear">AC</button>
      <button class="funcs" style="grid-area: plus-minus" @click="calculateToggle">±</button>
      <button class="funcs" style="grid-area: percent" @click="calculateDel">DEL</button>
      <button class="ops" style="grid-area: add" @click="append('+')">+</button>
      <button class="ops" style="grid-area: subtract" @click="append('-')">-</button>
      <button class="ops" style="grid-area: multiply" @click="append('×')">×</button>
      <button class="ops" style="grid-area: divide" @click="append('÷')">÷</button>
      <button class="ops" style="grid-area: equal" @click="cal">=</button>

      <button style="grid-area: number-1" @click="append(1)">1</button>
      <button style="grid-area: number-2" @click="append(2)">2</button>
      <button style="grid-area: number-3" @click="append(3)">3</button>
      <button style="grid-area: number-4" @click="append(4)">4</button>
      <button style="grid-area: number-5" @click="append(5)">5</button>
      <button style="grid-area: number-6" @click="append(6)">6</button>
      <button style="grid-area: number-7" @click="append(7)">7</button>
      <button style="grid-area: number-8" @click="append(8)">8</button>
      <button style="grid-area: number-9" @click="append(9)">9</button>
      <button style="grid-area: number-0" @click="append(0)">0</button>

      <button style="grid-area: dot" @click="append('.')">.</button>
    </div>
  </div>
</template>

<script>
export default({
  el: '#app',
  name: 'App',
  data() {
    return {
      eq: '0',
      isDecAdded: false,
      isOpAdded: false,
      isStarted: false,
    }
  },
  methods: {
    isOperator(character) {
      if(character == '+' || character == '-' || character == '×' || character == '÷') return true;
      return false;
    },
    // When pressed Operators or Numbers
    append(character) {
      // Start
      if (this.eq === '0' && !this.isOperator(character)) {
        if (character === '.') {
          this.eq += '' + character
          this.isDecAdded = true
        } else {
          this.eq = '' + character
        }
        
        this.isStarted = true
        return
      }
      
      // If Number
      if (!this.isOperator(character)) {
        if (character === '.' && this.isDecAdded)  return
        
        if (character === '.') {
          this.isDecAdded = true
          this.isOpAdded = true
        } else {
          this.isOpAdded = false
        }
        
        this.eq += '' + character
      }
      
      // Added Operator
      if (this.isOperator(character) && !this.isOpAdded) {
        this.eq += '' + character
        this.isDecAdded = false
        this.isOpAdded = true
      }
    },
    // When pressed '='
    cal() {
      let result = this.eq.replace(new RegExp('×', 'g'), '*').replace(new RegExp('÷', 'g'), '/')
      
      this.eq = parseFloat(eval(result).toFixed(9)).toString()
      this.isDecAdded = false
      this.isOpAdded = false
    },
    // When pressed '+/-'
    calculateToggle() {
      if (this.isOpAdded || !this.isStarted) return
      this.eq = this.eq + '* -1'
      this.cal()
    },
   
    // When pressed 'DEL'
    calculateDel() {
      if (this.isOpAdded || !this.isStarted) return
      if(this.eq.length == 1)this.eq = "0";
      else this.eq = this.eq.substring(0,this.eq.length-1);
    },


    // When pressed 'AC'
    clear() {
      this.eq = '0'
      this.isDecAdded = false
      this.isOpAdded = false
      this.isStarted = false
    }
  }
})
</script>

<style>
body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: white;
 
}

.calculator {
  --button-width: 80px;
  --button-height: 80px;
  
  display: grid;
  grid-template-areas: "result result result result"
    "ac plus-minus percent divide"
    "number-7 number-8 number-9 multiply"
    "number-4 number-5 number-6 subtract"
    "number-1 number-2 number-3 add"
    "number-0 number-0 dot equal";
  grid-template-columns: repeat(4, var(--button-width));
  grid-template-rows: repeat(6, var(--button-height));
  background-color: #ededed;
  


}

.calculator button {
  
  padding: 0;
  border: 0;
  display: block;
  outline: none;
  font-size: 24px;
  font-family: Helvetica;
  font-weight: normal;
  background-color: #ededed;
  color:black;
  
}
.calculator button:hover{
  background: linear-gradient(135deg, rgb(28, 175, 65) 0%, rgb(28, 175, 35) 100%);
}
.calculator button:active {
  box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1) inset, 4px 4px 10px -8px rgba(0, 0, 0, .3) inset;
}
.calculator button .funcs{
  
  background-color: #ededed;
  color:black;
  
}
.calculator button .ops{
  
background-color: #ff7d19;
  color:black;
  
}
.result {
  text-align: right;
  line-height: var(--button-height);
  font-size: 48px;
  font-family: Helvetica;
  white-space: nowrap;
  overflow-x: auto;
  overflow-y: hidden;
  background-color: black;
  color:white;
}
</style>
