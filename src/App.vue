<template>
  <div id="app">
    <div class="calculator">
      <History v-bind:history="history"></History>
      <ExpressionDisplay
              v-bind:expression="this.expression"
              v-bind:result="expressionResult">
      </ExpressionDisplay>
      <input type="text" class="calculator-display" v-model="display" disabled />
      <Keypad @btn-clicked="btnClicked"></Keypad>
    </div>
  </div>
</template>

<script>
import Keypad from './components/Keypad.vue';
import ExpressionDisplay from './components/ExpressionDisplay.vue';
import History from './components/History.vue';

const STATES = Object.freeze({
  typingDigit: 'typingDigit',
  operatorClicked: 'operatorClicked',
  equalClicked: 'equalClicked',
  clearClicked: 'clearClicked',
});

export default {
  name: 'app',
  data() {
    return {
      expression: [],
      display: '0',
      pendingOperator: null,
      state: null,
      history: [],
    };
  },
  methods: {
    btnClicked(group, btn) {
      console.log('[btn click]', { group, btn });
      switch (group) {
        case 'digit': {
          this.handleDigit(btn);
          this.state = STATES.typingDigit;
          break;
        }
        case 'decimal': {
          // Ignore if typing decimal immediately after equal
          // Ignore if display already contained decimal point
          if (this.state === STATES.equalClicked || this.display.includes('.')) {
            return;
          }
          this.handleDecimal();
          this.state = STATES.typingDigit;
          break;
        }
        case 'operator': {
          this.handleOperator(btn);
          this.state = STATES.operatorClicked;
          break;
        }
        case 'equal': {
          // Allow only if typing equal immediately after digit
          // Ignore if there is no expression to take (no pending operator)
          if (this.state !== STATES.typingDigit || !this.pendingOperator) {
            return;
          }
          this.handleEqual();
          this.state = STATES.equalClicked;
          break;
        }
        case 'clear': {
          this.handleClear();
          this.state = STATES.clearClicked;
          break;
        }
        default: {
          throw new Error(`Unknown click group: ${group}`);
        }
      }
    },
    handleDigit(digitValue) {
      // Ignore if typing digit immediately after equal
      if (this.state === STATES.equalClicked) {
        this.display = digitValue;
        this.pushToHistory();
        this.expression = [];
      } else if (this.state === STATES.operatorClicked) {
        // if digit after operator, replace the display
        this.display = digitValue;
      } else {
        // otherwise concat the digit
        this.display = this.display === '0' ? digitValue : this.display + digitValue;
      }
    },
    handleDecimal() {
      if (this.state === STATES.operatorClicked) {
        // if after operator, replace the display
        this.display = '.';
      } else {
        // otherwise concat
        this.display = this.display === '0' ? '.' : `${this.display}.`;
      }
    },
    handleOperator(operator) {
      if (this.state === STATES.typingDigit) {
        this.updateExpression(this.display);
      }
      if (this.state === STATES.equalClicked) {
        this.pushToHistory();
        this.expression = [this.display];
      }
      this.pendingOperator = operator;
    },
    handleEqual() {
      this.updateExpression(this.display);
      this.display = this.expressionResult.toString();
    },
    handleClear() {
      this.display = '0';
      this.expression = [];
      this.state = null;
      this.pendingOperator = null;
    },
    updateExpression(operand) {
      if (this.pendingOperator) {
        this.expression.push(this.pendingOperator);
      } else if (this.expression.length !== 0) {
        // push with no pending operator is only allowed if this is the first push to the expression
        throw new Error('Could not update expression, missing pending operator');
      }
      this.expression.push(parseFloat(operand));
      this.pendingOperator = null;
    },

    pushToHistory() {
      this.history.push(`${this.expression.join(' ')} = ${this.expressionResult}`);
    },
  },
  computed: {
    expressionResult() {
      // eslint-disable-next-line no-eval
      return this.expression.length > 0 ? eval(this.expression.join(' ')) : 0;
    },
  },
  components: {
    Keypad, ExpressionDisplay, History,
  },
};
</script>

<style type="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}

.calculator {
  border: 1px solid #ccc;
  border-radius: 5px;
  max-width: 400px;
  margin: 0 auto;
}

.calculator-display {
  width: 100%;
  font-size: 5rem;
  height: 80px;
  border: none;
  background-color: #252525;
  color: #fff;
  text-align: right;
  display: block;
  padding: 0 10px;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  background-image: none;
  -webkit-box-shadow: none;
  -moz-box-shadow: none;
  box-shadow: none;
}
</style>
