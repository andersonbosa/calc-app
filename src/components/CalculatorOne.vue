<template>
  <div>
    <div class="calculator">
      <div class="display">{{ currentValue || "0" }}</div>
      <div @click="clearDisplay" class="btn">C</div>
      <div @click="invertMathSign" class="btn">+/-</div>
      <div @click="percentOperation" class="btn">%</div>
      <div @click="divideOperation" class="btn operator">÷</div>
      <div @click="appendValue('7')" class="btn">7</div>
      <div @click="appendValue('8')" class="btn">8</div>
      <div @click="appendValue('9')" class="btn">9</div>
      <div @click="timesOperation" class="btn operator">x</div>
      <div @click="appendValue('4')" class="btn">4</div>
      <div @click="appendValue('5')" class="btn">5</div>
      <div @click="appendValue('6')" class="btn">6</div>
      <div @click="minusOperation" class="btn operator">-</div>
      <div @click="appendValue('1')" class="btn">1</div>
      <div @click="appendValue('2')" class="btn">2</div>
      <div @click="appendValue('3')" class="btn">3</div>
      <div @click="addOperation" class="btn operator">+</div>
      <div @click="appendValue('0')" class="btn zero">0</div>
      <div @click="appendDot" class="btn">.</div>
      <div @click="equalOperation" class="btn operator">=</div>
    </div>
    <div class="feedbacks">
      <br />
      <input type="text" placeholder="actions feedback" :value="feedbackMessage" disabled />
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalculatorOne',

  data () {
    return {
      feedbackMessage: '',
      previousValue: null,
      currentValue: '0',
      operator: null,
      operatorClicked: false
    }
  },

  methods: {
    cleanFeedback () {
      this.feedbackMessage = ''
    },

    updateFeedback (msg) {
      this.feedbackMessage = String(msg)
      setTimeout(() => {
        this.cleanFeedback('')
      }, 4000)
    },

    clearDisplay () {
      this.currentValue = ''
    },

    invertMathSign () {
      this.currentValue = this.currentValue.charAt(0) === '-'
        ? this.currentValue.slice(1) : `-${this.currentValue}`
    },

    percentOperation () {
      this.currentValue = `${parseFloat(this.currentValue) / 100}`
    },

    appendValue (number) {
      if (this.operatorClicked) {
        this.currentValue = ''
        this.operatorClicked = false
      }
      this.currentValue = `${this.currentValue}${number}`
    },

    appendDot () {
      const hasDot = this.currentValue.includes('.')
      if (hasDot) {
        return
      }
      this.currentValue = `${this.currentValue.slice(0, 1)}.${this.currentValue.slice(1)}`
    },

    setPrevious () {
      this.previousValue = this.currentValue
      this.operatorClicked = true
    },

    divideOperation () {
      this.operator = (a, b) => a / b
      this.setPrevious()
    },

    timesOperation () {
      this.operator = (a, b) => a * b
      this.setPrevious()
    },

    minusOperation () {
      this.operator = (a, b) => a - b
      this.setPrevious()
    },

    addOperation () {
      this.operator = (a, b) => a + b
      this.setPrevious()
    },

    equalOperation () {
      this.currentValue = `${this.operator(
        parseFloat(this.currentValue),
        parseFloat(this.previousValue)
      )}`
      this.previousValue = null
    },

    triggerShortcut (evt) {
      const shortcutMap = {
        Backspace: () => this.clearDisplay(),
        '%': () => this.percentOperation(),
        '/': () => this.divideOperation(),
        7: () => this.appendValue(7),
        8: () => this.appendValue(8),
        9: () => this.appendValue(9),
        '*': () => this.timesOperation(),
        4: () => this.appendValue(4),
        5: () => this.appendValue(5),
        6: () => this.appendValue(6),
        '-': () => this.minusOperation(),
        1: () => this.appendValue(1),
        2: () => this.appendValue(2),
        3: () => this.appendValue(3),
        '+': () => this.addOperation(),
        0: () => this.appendValue(0),
        ',': () => this.appendDot(),
        '.': () => this.appendDot(),
        Enter: () => this.equalOperation()
      }

      const isMapped = shortcutMap[evt.key]
      if (isMapped) {
        isMapped()
        this.updateFeedback(`pressed ${evt.key}`)
      }
    },

    bindShortcuts () {
      window.document.addEventListener('keyup', evt => this.triggerShortcut(evt))
      window.document.querySelector('.calculator').addEventListener('click', evt => {
        this.updateFeedback(`clicked in ${evt.target.innerText}`)
      })
    }
  },
  mounted () {
    this.bindShortcuts()
  }
}
</script>

<style scoped>
.calculator {
  text-align: center;
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  background-color: #f2f2f2;
  border: 1px solid #999;
}

.operator {
  background-color: orange;
  color: white;
}
.feedbacks input {
  font-size: 12px;
  color: gray;
  border: 1px solid gray;
  border-radius: 5px;
  padding: 2px 5px;
  text-align: center;
}
</style>
