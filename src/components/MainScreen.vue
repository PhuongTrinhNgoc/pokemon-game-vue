<template>
  <div class="screen">
    <h1>POKE MEMORIES</h1>
    <h2>Select mode to start game</h2>
    <div class="action">
      <button @click="onStart(16)">
        <span>4x4</span>
        <span>easy</span>
      </button>
      <button @click="onStart(36)">
        <span>6x6</span>
        <span>normal</span>
      </button>
      <button @click="onStart(64)">
        <span>8x8</span>
        <span>hard</span>
      </button>
      <button @click="onStart(100)">
        <span>10x10</span>
        <span>super hard</span>
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: "MainApp",
  methods: {
    onStart(totalOfBlock) {
      this.$emit("onStart", { totalOfBlock });
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);
}

.screen h1 {
  font-size: 4.5rem;
  text-transform: uppercase;
}

.screen p {
  font-size: 2rem;
}

.action {
  display: flex;
  margin-top: 2rem;
}

.action button {
  font: var(--font);
  width: 150px;
  height: 150px;
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

.action button:hover {
  background-color: var(--light);
  color: var(--dark);
}

.action button span:first-child {
  font-size: 2rem;
}

.action button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
.screen h2 {
  font-size: 30px;
}
</style>
