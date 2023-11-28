<template>
  <div>
    <main-screen v-if="statusMatch === 'default'" @onStart="onHandle($event)" />
    <interact-screen
      v-if="statusMatch === 'match'"
      :cardsContext="setting.cardsContext"
      :cardLength="cardLength"
      @onFinish="onGetResult"
      :statusMatch="statusMatch"
      @backButtonClick="onBackButtonClickHandler"
    />
    <result-screen
      v-if="statusMatch === 'result'"
      :timer="timer"
      @onStartAgain="statusMatch = 'default'"
    />
    <div class="coppy">
      <copyy-by-screen />
    </div>
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CoppyRightScreen from "./components/CoppyRightScreen.vue";

import { shuffled } from "./utils/array";
export default {
  name: "App",
  data() {
    return {
      setting: {
        totalOfBlock: 0,
        cardsContext: [],
        started: null,
      },
      statusMatch: "default",
      cardLength: 0,
      timer: 0,
    };
  },
  components: {
    "main-screen": MainScreen,
    "interact-screen": InteractScreen,
    "result-screen": ResultScreen,
    "copyy-by-screen": CoppyRightScreen,
  },
  methods: {
    onHandle(config) {
      this.setting.totalOfBlock = config.totalOfBlock;
      const firsCard = Array.from(
        { length: this.setting.totalOfBlock / 2 },
        (_, i) => i + 1
      );
      const seconCard = [...firsCard];
      const cards = [...firsCard, ...seconCard];
      // console.log(cards);
      this.setting.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));
      // console.log(this.setting.cardsContext);

      this.setting.started = new Date().getTime();
      this.statusMatch = "match";
      this.cardLength = firsCard.length;
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.setting.started;
      this.statusMatch = "result";
    },
    onBackButtonClickHandler() {
      this.statusMatch = "default";
    },
  },
};
</script>

<style>
.coppy {
  position: absolute;
  z-index: 1000;
  bottom: 10px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 26px;
}
.coppy p {
  color: var(--light);
}
.coppy p a {
  color: #f4dc26;
}
</style>
