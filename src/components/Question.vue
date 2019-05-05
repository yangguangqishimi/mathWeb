<template>
  <div class="alert">
    <h3>{{ method === '÷' ? x * y : method === '-' ? x + y : x }} {{ method }} {{ y }} = ?</h3>
    <hr>
    <div class="buttons">
      <button class="btn btn-success" v-for="number in answers" @click="onAnswer(number)">{{number}}</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["settings"],
  data() {
    return {
      x: mtRand(this.settings.from, this.settings.to),
      y: mtRand(this.settings.from, this.settings.to),
      method: this.settings.method
    };
  },
  computed: {
    good() {
      return this.method === "+"
        ? this.x + this.y
        : this.method === "x"
        ? this.x * this.y
        : this.x;
    },
    answers() {
      let res = [this.good];
      while (res.length < this.settings.variants) {
        var num = mtRand(
          this.good - this.settings.range,
          this.good + this.settings.range
        );
        
        if (this.good > 30 && this.settings.range > 20) {
          let t1 = this.good % 10;
          num = num - num % 10 + t1;
        }
        if (num > 0 && res.indexOf(num) === -1) {
          res.push(num);
        }
      }
      if (this.settings.range <= 10 && res[1] > 10) {
        res[1] = res[0] + Math.random() > 0.5 ? 10 : -10;
      }
      return randomSort(res);
    }
  },
  methods: {
    onAnswer(num) {
      if (num == this.good) {
        this.$emit("success");
      } else {
        this.$emit(
          "error",
          `${
            this.method === "÷"
              ? this.x * this.y
              : this.method === "-"
              ? this.x + this.y
              : this.x
          } ${this.method} ${this.y}=${this.good} !`
        );
      }
    }
  }
};

function mtRand(min, max) {
  let diff = max - min;
  return Math.floor(Math.random() * (diff + 1)) + min;
}

function randomSort(arr) {
  for (let i = 0; i < 10; i++) {
    const r1 = Math.floor(Math.random() * 4);
    const r2 = Math.floor(Math.random() * 4);
    const tmp = arr[r2];
    arr[r2] = arr[r1];
    arr[r1] = tmp;
  }
  return arr;
}
</script>

<style scoped>
.alert {
  padding-top: 20px;
  background-color: #eee;
}

.buttons {
  display: flex;
  justify-content: space-around;
}

.btn {
  margin: 20px 0;
}
</style>