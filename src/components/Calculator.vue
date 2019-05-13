<template>
  <div class="calculator">
    <!-- nav buttons -->
    <div class="nav-btns">
      <div class="nav-btn" @click="onLink('https://www.linkedin.com/in/mustapha-aouas-7918a214b/')"></div>
      <div class="nav-btn" @click="onLink('https://www.doyoubuzz.com/mustapha-aouas')"></div>
      <div class="nav-btn" @click="onLink('https://github.com/ultraMIND/')"></div>
    </div>
    <!-- window -->
    <div class="window">
      <span>{{ result }}</span>
    </div>
    <!-- row 1 -->
    <div class="btn function" v-if="operand[0]" @click="onCorrect()">
      <span>C</span>
    </div>
    <div class="btn function" v-else-if="!operand[0]" @click="onReset()">
      <span>AC</span>
    </div>
    <div class="btn function" @click="onPlusMinus()">
      <img src="../assets/mathematical-basic-signs-of-plus-and-minus-with-a-slash.svg">
    </div>
    <div class="btn function" @click="onPercent()">
      <img src="../assets/percent-sign.svg">
    </div>
    <div
      class="btn operator"
      :class="{ 'selected': operator == operators.divide}"
      @click="onSelectOp(operators.divide)"
    >
      <img src="../assets/divide.svg">
    </div>
    <!-- row 2 -->
    <div class="btn" @click="onNumber(7)">
      <span>7</span>
    </div>
    <div class="btn" @click="onNumber(8)">
      <span>8</span>
    </div>
    <div class="btn" @click="onNumber(9)">
      <span>9</span>
    </div>
    <div
      class="btn operator"
      :class="{ 'selected': operator == operators.multiply}"
      @click="onSelectOp(operators.multiply)"
    >
      <img
        style="transform: rotate(45deg)"
        src="../assets/plus-positive-add-mathematical-symbol.svg"
      >
    </div>
    <!-- row 3 -->
    <div class="btn" @click="onNumber(4)">
      <span>4</span>
    </div>
    <div class="btn" @click="onNumber(5)">
      <span>5</span>
    </div>
    <div class="btn" @click="onNumber(6)">
      <span>6</span>
    </div>
    <div
      class="btn operator"
      :class="{ 'selected': operator == operators.minus}"
      @click="onSelectOp(operators.minus)"
    >
      <img src="../assets/minus-sign.svg">
    </div>
    <!-- row 4 -->
    <div class="btn" @click="onNumber(1)">
      <span>1</span>
    </div>
    <div class="btn" @click="onNumber(2)">
      <span>2</span>
    </div>
    <div class="btn" @click="onNumber(3)">
      <span>3</span>
    </div>
    <div
      class="btn operator"
      :class="{ 'selected': operator == operators.plus}"
      @click="onSelectOp(operators.plus)"
    >
      <img src="../assets/plus-positive-add-mathematical-symbol.svg">
    </div>
    <!-- row 5 -->
    <div class="btn zero" @click="onNumber(0)">
      <span>0</span>
    </div>
    <div class="btn" @click="onComma()">
      <span>,</span>
    </div>
    <div class="btn operator" @click="onEqual()">
      <img src="../assets/equal-mathematical-sign.svg">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      operators: {
        plus: 'plus',
        minus: 'minus',
        multiply: 'multiply',
        divide: 'divide',
      },
      operator: null,
      operand: [null, null],
    };
  },
  computed: {
    result() {
      return this.operand[0] ? this.operand[0].split('.').join(',') : '0';
    },
  },
  methods: {
    onNumber(n) {
      if (this.operator && !this.operand[1]) {
        this.operand[1] = this.operand[0];
        this.operand[0] = String(n);
      } else {
        const str =
          this.operand[0] && !isNaN(this.operand[0]) ? this.operand[0] : '';
        this.operand[0] = str + n;
      }
      this.refresh();
    },
    onComma() {
      if (this.operator && !this.operand[1]) {
        this.operand[1] = this.operand[0];
        this.operand[0] = String('0.');
      } else if (this.operand[0] && this.operand[0].includes('.')) {
        return;
      } else {
        const str =
          this.operand[0] && !isNaN(this.operand[0]) ? this.operand[0] : '0';
        this.operand[0] = str + '.';
      }
      this.refresh();
    },
    plus() {
      this.operand[0] = String(+this.operand[1] + +this.operand[0]);
      this.operand[1] = null;
      this.refresh();
    },
    minus() {
      this.operand[0] = String(+this.operand[1] - +this.operand[0]);
      this.operand[1] = null;
      this.refresh();
    },
    multiply() {
      this.operand[0] = String(+this.operand[1] * +this.operand[0]);
      this.operand[1] = null;
      this.refresh();
    },
    divide() {
      if (this.operand[0] == 0) {
        this.operand[0] = NaN;
      } else {
        this.operand[0] = String(+this.operand[1] / +this.operand[0]);
      }
      this.operand[1] = null;
      this.refresh();
    },
    refresh() {
      this.operand = [...this.operand];
    },
    onSelectOp(op) {
      if (!this.operand[0]) {
        this.operand[0] = '0';
      }
      if (this.operator && this.operand[1]) {
        this[this.operator]();
      }
      this.operator = op;
    },
    onPercent() {
      this.operand[0] = String(+this.operand[0] / 100);
      this.refresh();
    },
    onPlusMinus() {
      this.operand[0] = String(-+this.operand[0]);
      this.refresh();
    },
    onEqual() {
      this.onSelectOp(null);
    },
    onReset() {
      this.operator = null;
      this.operand = [null, null];
    },
    onCorrect() {
      if (!this.operator && !this.operand[1]) {
        this.onReset();
      } else if (this.operator && this.operand[1]) {
        this.operand[0] = null;
        this.refresh();
      }
    },
    onLink(link) {
      window.open(link, '_blank');
    },
  },
};
</script>

<style scoped>
.calculator {
  --width: 470px;
  --height: 650px;
  --window-height: 160px;
  --window-font-size: 75px;
  --font-size: 35px;
  --border-radius: 10px;
  --border-width: 1px;
  --btn-radius: 30px;
  --gap: 2px;

  transform: scale(0.7);
  margin: 0 auto;
  width: var(--width);
  height: var(--height);
  background-color: #31322e;
  border-radius: var(--border-radius);
  position: relative;
  color: white;
  font-size: var(--font-size);
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  grid-template-rows: var(--window-height) 1fr 1fr 1fr 1fr 1fr;
  grid-gap: var(--gap);
  border: var(--border-width) solid black;
  overflow: hidden;
  box-shadow: 0 19px 38px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);
}
.calculator > .nav-btns {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
}
.calculator > .nav-btns > .nav-btn {
  cursor: pointer;
  background-color: #ed655a;
  border-radius: 50%;
  width: var(--btn-radius);
  height: var(--btn-radius);
  margin: calc(var(--btn-radius) / 2.3);
}
.calculator > .nav-btns > .nav-btn:nth-of-type(2) {
  background: #e0c04c;
}
.calculator > .nav-btns > .nav-btn:nth-of-type(3) {
  background: #72bf47;
}
.calculator > .window {
  font-size: var(--window-font-size);
  grid-column: 1 / -1;
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
  padding: 0 var(--btn-radius) calc(var(--btn-radius) / 3);
}
.calculator > .window > span {
  overflow-y: hidden;
  overflow-x: auto;
  line-height: var(--window-font-size);
  max-width: 100%;
}
.calculator > .btn {
  cursor: pointer;
  background-color: #646362;
  padding: 0 38%;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: 175ms;
}
.calculator > .btn > img {
  max-width: 100%;
}
.calculator > .btn.function {
  background-color: #464442;
}
.calculator > .btn.operator {
  background-color: #f2a33c;
}
.calculator > .btn.zero {
  grid-column: span 2;
}
.calculator > .btn:active {
  background-color: #a3a1a1;
}
.calculator > .btn.function:active {
  background-color: #646362;
}
.calculator > .btn.operator:active {
  background-color: #c0812e;
}
.calculator > .btn.operator.selected {
  border: var(--border-width) solid black;
}
</style>
