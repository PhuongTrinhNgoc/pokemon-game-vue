<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFace="`img/${card}.png`"
        :card="{ index, value: card }"
        :rules="rules"
        @onFlip="checkRule($event)"
        :cardsContext="cardsContext"
      />
      <button @click="onBackButtonClick" class="back">Back</button>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    cardLength: {
      type: Number,
      default: 0,
    },
    statusMatch: {
      type: String,
    },
  },
  components: {
    "card-flip": Card,
  },
  data() {
    return {
      count: 0,
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      console.log(this.rules);
      const firstRule = this.rules[0];
      const secondRule = this.rules[1];
      if (this.rules.length === 2) {
        if (firstRule.value === secondRule.value) {
          const cardRef0 = this.$refs[`card-${firstRule.index}`][0];
          const cardRef1 = this.$refs[`card-${secondRule.index}`][0];
          cardRef0.onEnabDisable();
          cardRef1.onEnabDisable();
          this.count += 1;
          this.rules = [];
          if (this.count === this.cardLength) {
            setTimeout(() => {
              this.$emit("onFinish");
            }, 900);
          }
        } else {
          setTimeout(() => {
            const cardRef0 = this.$refs[`card-${firstRule.index}`][0];
            const cardRef1 = this.$refs[`card-${secondRule.index}`][0];

            cardRef0.onFlipBackCard();
            cardRef1.onFlipBackCard();
            this.rules = [];
          }, 800);
        }
      }
    },
    onBackButtonClick() {
      this.$emit("backButtonClick");
    },
  },
};
</script>

<style lang="css" scoped>
.back {
  position: fixed;
  top: 20px;
  left: 20px;
  font-size: 20px;
  width: 100px;
  height: 80px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}
.back:hover {
  background-color: var(--light);
  color: var(--dark);
}
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
  max-width: 800px; /* Adjust this value based on your needs */
  width: 100%;
}
</style>
