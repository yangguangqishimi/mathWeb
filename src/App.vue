<template>
  <div class="training">
    <h1>数学题训练营</h1>
    <div
      style="display:flex;
  flex-direction: row;
  justify-content: space-between
  width: 700px"
    >
      <h2><select name="selectAge" id="selectAge" @change="selectLevel">   
        <option value="1">加法1</option>   
        <option value="2">加法2</option>   
        <option value="3">加法3</option> 
        <option value="1">减法1</option>   
        <option value="2">减法2</option>   
        <option value="3">减法3</option>   
        <option value="4">乘法1</option>   
        <option value="5">乘法2</option>   
        <option value="6">乘法3</option>
        <option value="7">除法1</option>
        <option value="8">除法2</option>  
        <option value="9">除法3</option>
      </select></h2>
      <h2>分数: {{ score }} </h2>
    </div>

    <hr>
    <div class="progress">
      <div class="progress-bar" :style="progressStyle"></div>
    </div>
    <div class="box">
      <transition name="flip" mode="out-in">
        <app-start-screen v-if="state=='start'" @onStart="onStart"></app-start-screen>
        <app-question
          v-else-if="state=='question'"
          @success="onQuestSuccess"
          @error="onQuestError"
          :settings="levels[level]"
        ></app-question>
        <app-message
          v-else-if="state=='message'"
          :type="message.type"
          :text="message.text"
          @next="onNext"
        ></app-message>
        <app-result-screen
          v-else-if="state=='results'"
          :stats="stats"
          :level="level"
          @repeat="onStart"
          @eraseGame="onEraseGame"
          @nextLevel="onNextLevel"
        ></app-result-screen>
        <div v-else>Unknown state</div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      state: "start",
      stats: {
        success: 0,
        error: 0,
        time: 0
      },
      message: {
        type: "",
        text: ""
      },
      questMax: 20,
      level: 0,
      levels: [
        {
          from: 10,
          to: 40,
          range: 5,
          variants: 4,
          method: '+'
        },
        {
          from: 100,
          to: 200,
          range: 20,
          variants: 4,
          method: '+'
        },
        {
          from: 500,
          to: 900,
          range: 40,
          variants: 4,
          method: '+'
        },
        {
          from: 10,
          to: 40,
          range: 5,
          variants: 4,
          method: '-'
        },
        {
          from: 100,
          to: 200,
          range: 20,
          variants: 4,
          method: '-'
        },
        {
          from: 500,
          to: 900,
          range: 40,
          variants: 4,
          method: '-'
        },
        {
          from: 5,
          to: 20,
          range: 10,
          variants: 4,
          method: 'x'
        },
        {
          from: 10,
          to: 40,
          range: 100,
          variants: 4,
          method: 'x'
        },
        {
          from: 30,
          to: 100,
          range: 200,
          variants: 4,
          method: 'x'
        },
        {
          from: 5,
          to: 20,
          range: 5,
          variants: 4,
          method: '÷'
        },
        {
          from: 10,
          to: 40,
          range: 5,
          variants: 4,
          method: '÷'
        },
        {
          from: 30,
          to: 100,
          range: 10,
          variants: 4,
          method: '÷'
        }
      ],
      score: 0
    };
  },
  computed: {
    questDone() {
      return this.stats.success + this.stats.error;
    },
    progressStyle() {
      return {
        width: (this.questDone / this.questMax) * 100 + "%"
      };
    }
  },
  methods: {
    onStart() {
      this.state = "question";
      this.stats.success = 0;
      this.stats.error = 0;
      this.stats.time = Date.now();
      this.score = 0;
    },
    onQuestSuccess() {
      this.state = "message";
      this.message.text = "真聪明，答对了!";
      this.message.type = "success";
      this.stats.success++;
      this.score += 5;
    },
    onQuestError(msg) {
      this.state = "message";
      this.message.text = msg;
      this.message.type = "warning";
      this.stats.error++;
    },
    onNext() {
      if (this.questDone < this.questMax) {
        this.state = "question";
      } else {
        this.stats.time = ((Date.now() - this.stats.time) / 60000).toFixed(1);
        this.state = "results";
      }
    },
    onNextLevel() {
      this.level++;
      this.onStart();
    },
    onEraseGame() {
      // this.level = 0;
      this.score = 0;
      this.onStart();
    },
    selectLevel(e) {
      this.level = e.target.selectedIndex
      this.score = 0;
      this.state = 'start'
    }
  }
};
</script>

<style scoped>
.training {
  max-width: 700px;
  margin: 20px auto;
}
.box {
  margin: 10px 0;
}
.flip-enter {
}
.flip-enter-active {
  animation: 0.3s flipInX;
}
.flip-leave {
}
.flip-leave-active {
  animation: 0.3s flipOutX;
}
@keyframes flipInX {
  from {
    transform: rotateX(90deg);
  }
  to {
    transform: rotateX(0deg);
  }
}
@keyframes flipOutX {
  from {
    transform: rotateX(0deg);
  }
  to {
    transform: rotateX(90deg);
  }
}
</style>
