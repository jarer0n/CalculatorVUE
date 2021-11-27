<template>
  <div class="container">
    <div class="calc_title">Калькулятор Vue 3</div>
    <div class="calc_row">
      <div class="calc_wrapper">
        <div class="calc_background"></div>
        <div class="calc_background2"></div>
        <div class="calc">
          <input
            @keyup.enter="mathResult()"
            class="calc_input"
            v-model="result"
            type="text"
            placeholder="0"
          />
          <button
            @click="inputValue(op)"
            v-for="op in operations"
            :key="op"
            class="calc_btn btn-op"
          >
            <span class="calc_btn-item">{{ op }}</span>
          </button>
          <button
            @click="inputValue(num)"
            v-for="num in numbers"
            class="calc_btn"
            :key="num"
          >
            <span class="calc_btn-item">{{ num }}</span>
          </button>
          <button @click="mathResult" class="calc_btn btn-equals">
            <span class="calc_btn-item">=</span>
          </button>
          <button @click="reset" class="calc_btn btn-op">
            <span class="calc_btn-item">Сбросить</span>
          </button>
          <button @click="saveResult" class="calc_btn btn-op">
            <span class="calc_btn-item">Запомнить</span>
          </button>
        </div>
      </div>
      <stored-values @removeStore="removeStore" :stored="stored" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: "",
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, "."],
      operations: ["+", "-", "*", "/", "(", ")"],
      stored: [],
      temp: "",
    };
  },
  watch: {
    result: function (val) {
      if (val) {
        this.result = String(val)
          .match(/[0-9]+|[\.|\*|\+|\/|\-\(\)]/g)
          .map((i) => this.regex(i))
          .join("");
      }
    },
  },
  methods: {
    inputValue(event) {
      this.result = this.result.toString();
      this.result += event;
    },
    reset() {
      this.result = "";
    },
    mathResult() {
      this.result = eval(this.result);
    },
    saveResult() {
      if (this.result !== "")
        this.stored.push({
          result: this.result,
          data: new Date().toLocaleTimeString(),
          math: "",
        });
    },
    removeStore(st) {
      this.stored = this.stored.filter((s) => s !== st);
    },

    regex(item) {
      let res = "";
      if (item == "." && item == this.temp) {
        res = "";
      } else if (
        ["*", "/", "-", "+"].indexOf(item) > -1 &&
        ["*", "/", "-", "+"].indexOf(this.temp) > -1
      ) {
        res = "";
      } else {
        res = item;
      }
      this.temp = item;
      return res;
    },
  },
};
</script>

<style lang="scss">
button {
  display: flex;
  align-items: center;
  justify-content: center;
}
.calc_title {
  font-size: 3.5rem;
  text-shadow: 2px 1px 2px black;
  padding: 40px;
  color: $yellow;
  text-align: center;
  margin-bottom: 2rem;
}
.calc_row {
  display: flex;
  justify-content: space-between;
}
.calc {
  display: flex;
  flex-wrap: wrap;
  padding: 3.5rem 2rem 2rem 2rem;
  background: rgba(255, 255, 255, 0.096);
  border-radius: 3rem;
  border-top: 1px solid rgba(255, 255, 255, 0.5);
  border-left: 1px solid rgba(255, 255, 255, 0.5);
  box-shadow: 20px 20px 50px rgba(0, 0, 0, 0.568);
  position: relative;
  z-index: 100;
  backdrop-filter: blur(5px);

  &_wrapper {
    position: relative;
    flex: 0 1 45rem;
    z-index: 100;
    margin-bottom: 5rem;
  }
  &_background,
  &_background2 {
    content: "";
    position: absolute;
    width: 18rem;
    height: 18rem;
    transform: rotate(25deg);
    background: $yellow;
    box-shadow: 5px 10px 40px rgba(0, 0, 0, 0.568);
  }
  &_background {
    bottom: 20%;
    left: -15%;
  }
  &_background2 {
    top: 15%;
    right: -10%;
    transform: rotate(70deg);
    background: rgb(247, 91, 91);
  }

  &_input {
    display: block;
    width: 100%;
    background: $main;
    text-align: right;
    font-weight: inherit;
    font-size: 2.8rem;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
    margin-bottom: 1rem;
  }
  &_btn {
    position: relative;
    flex: 25.33%;
    background: darken($color: $main, $amount: 35%);
    padding: 0.6em 0;
    font-size: 2.6rem;
    border-radius: 1em;
    margin: 0.3em;
    box-shadow: 0em 0.1em 0.1em rgba(0, 0, 0, 0.699);
    &-item {
      display: block;
      position: relative;
      z-index: 0;
      transform: translateY(-0.25em);
      transition: all 0.1s ease;
      color: black;
      font-weight: 900;
    }

    &:hover::before {
      background: lighten($color: $main, $amount: 15%);
    }
    &::before {
      content: "";
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: $main;
      border: 1px solid $main;
      border-radius: 1em;
      transform: translateY(-0.25em);
      transition: all 0.1s ease;
    }
    &:active::before {
      transform: translateY(0);
    }
    &:active .calc_btn-item {
      transform: translateY(0);
    }
  }
}
.btn-equals {
  &:hover::before {
    background: darken($color: rgb(247, 91, 91), $amount: 5%);
  }
  &::before {
    background: rgb(247, 91, 91);
    border: 1px solid rgba(0, 0, 0, 0.553);
  }
}

.btn-op {
  flex: 10%;
  padding: 0.2em;
  font-size: 1.4em;
  border-radius: 0.5em;
  background: rgb(29, 58, 86);
  transition: all 0.3s ease;
  margin: 0.8em 0.2em;
  &:hover {
    box-shadow: 0em 0.1em 0.1em darken($color: $yellow, $amount: 10%);
  }

  &::before {
    transform: translateY(-0.2em);
    background: $yellow;
    border: 1px solid rgb(29, 58, 86);
    transition: all 0.3s ease;
    border-radius: 0.5em;
  }
  &:hover::before {
    background: darken($color: $yellow, $amount: 10%);
  }
  span {
    transform: translateY(-0.3em);
  }
}
@media (max-width: 776px) {
  .calc_row {
    flex-direction: column;
  }
}
</style>
