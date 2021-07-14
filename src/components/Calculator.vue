<template>
  <div class="calculator-wrapper">
    <div class="calculator-screen">
      <div class="workspace">
        {{ current || 0 }}
      </div>
      <div
        v-if="current"
        class="result"
      >
        {{ result }}
      </div>
    </div>
    <div
      class="button basic-btn"
      @click="clear"
    >
      AC
    </div>
    <div
      class="button basic-btn"
      @click="sign"
    >
      +/-
    </div>
    <div
      class="button basic-btn"
      @click="percent"
    >
      %
    </div>
    <div
      class="button operator-btn"
      @click="divide"
    >
      &divide;
    </div>
    <div
      class="button number-btn"
      @click="append('7')"
    >
      7
    </div>
    <div
      class="button number-btn"
      @click="append('8')"
    >
      8
    </div>
    <div
      class="button number-btn"
      @click="append('9')"
    >
      9
    </div>
    <div
      class="button operator-btn"
      @click="times"
    >
      &times;
    </div>
    <div
      class="button number-btn"
      @click="append('4')"
    >
      4
    </div>
    <div
      class="button number-btn"
      @click="append('5')"
    >
      5
    </div>
    <div
      class="button number-btn"
      @click="append('6')"
    >
      6
    </div>
    <div
      class="button operator-btn"
      @click="minus"
    >
      -
    </div>
    <div
      class="button number-btn"
      @click="append('1')"
    >
      1
    </div>
    <div
      class="button number-btn"
      @click="append('2')"
    >
      2
    </div>
    <div
      class="button number-btn"
      @click="append('3')"
    >
      3
    </div>
    <div
      class="button operator-btn"
      @click="add"
    >
      +
    </div>
    <div
      class="button zero number-btn"
      @click="append('0')"
    >
      0
    </div>
    <div
      class="button number-btn"
      @click="dot"
    >
      .
    </div>
    <div
      class="button operator-btn"
      @click="equal"
    >
      =
    </div>
  </div>
</template>

<script>
const removeLastZero = (stringNumber) => {
  const lastIndex = stringNumber.length - 1;
  if (lastIndex === -1) return stringNumber;
  if (stringNumber[lastIndex] !== "0") return stringNumber;
  let isRemove = true;

  for (let i = lastIndex - 1; i > -1; i--) {
    if (stringNumber[i].search(/[+\-*/]/g) >= 0) break;
    if (stringNumber[i] !== "0") isRemove = false;
  }

  return isRemove ? stringNumber.slice(0, lastIndex) : stringNumber;
};

export default {
  name: "Calculator",
  data() {
    return {
      current: "",
    };
  },
  computed: {
    result: function () {
      const length = this.current.length;
      const output =
        this.current[length - 1].search(/[/*\-+]/g) > -1
          ? this.current.slice(0, length - 1)
          : this.current;
      return eval(output)?.toString() || 0;
    },
  },
  methods: {
    clear() {
      this.current = "";
    },
    sign() {
      if (this.current && this.current != "0")
        this.current =
          this.current.charAt(0) === "-"
            ? this.current.slice(1)
            : `-${this.current}`;
    },
    append(value) {
      if (value.search(/\d/g) >= 0) {
        this.current = removeLastZero(this.current);
      }
      this.current = `${this.current}${value}`;
    },
    percent() {
      this.equal();
      this.current = (+this.current / 100).toString();
    },
    dot() {
      const input = this.current.split(/[*\-+/]/g);
      if (input[input.length - 1].indexOf(".") < 0) {
        this.append(".");
      }
    },
    checkCurrent() {
      this.current =
        this.current.search(/[*\-+/]/g) >= 0 ? this.result : this.current;
    },
    add() {
      this.checkCurrent();
      this.append("+");
    },
    minus() {
      this.checkCurrent();
      this.append("-");
    },
    divide() {
      if (this.current) {
        this.checkCurrent();
        this.append("/");
      }
    },
    times() {
      if (this.current) {
        this.checkCurrent();
        this.append("*");
      }
    },
    equal() {
      this.current = this.result;
    },
  },
};
</script>

<style>
.calculator-wrapper {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: 1.5fr repeat(5, 1fr);
  gap: 1px;

  min-width: 358px;
  min-height: 500px;

  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  color: rgba(255, 255, 255, 0.8);
  background: #2b2d2f;
  overflow: hidden;
  border-radius: 5px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 1.5em;
  font-weight: 500;
}
.calculator-wrapper .calculator-screen {
  grid-column: span 4;
  text-align: right;
  background: #2b2d2f;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: flex-end;
  padding: 14px;
}

.basic-btn {
  background: #3f4143;
}
.operator-btn {
  background: #ff9f0c;
}
.number-btn {
  background: #5f6062;
}
.calculator-wrapper .zero {
  grid-column: span 2;
}

.button {
  outline: none;
  border: none;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: ease-in-out 0.1;
}
.button:hover {
  opacity: 0.9;
  box-shadow: 0 0 6px black;
}
</style>