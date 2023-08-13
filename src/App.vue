<template>
  <div id="app">
    <div class="container">
      <button @click="onIncrease">Increase</button>
      <span class="counter">{{ count }}</span>
      <button @click="onDecrease">Decrease</button>
    </div>

    <div class="container action">
      <button :disabled="!isActionButtonsEnabled">Undo</button
      ><button :disabled="!isActionButtonsEnabled">Redo</button>
    </div>
  </div>

  <span v-if="actions"> Last performed Action : {{ actions.action }}</span>
  <watch-effect-demo :shouldFetch="true"></watch-effect-demo>
</template>

<script lang="ts">
import { ref, computed, watch, watchEffect } from 'vue';
import WatchEffectDemo from './components/WatchEffectDemo.vue';

export default {
  name: 'App',
  components: { WatchEffectDemo },

  setup() {
    const count = ref<number>(0);
    const actions = ref<{ action: string; count: number } | null>(null);

    const isActionButtonsEnabled = computed(() => {
      return actions.value;
    });

    const onIncrease = () => {
      const prevCount = count.value;

      count.value = count.value + 1;

      const actionObject = {
        action: 'Increment',
        count: prevCount,
      };

      actions.value = actionObject;
    };

    const onDecrease = () => {
      const prevCount = count.value;

      count.value = count.value - 1;

      const actionsObject = {
        action: 'Decrement',
        count: prevCount,
      };

      actions.value = actionsObject;
    };

    watchEffect(() => {
      console.log('Actions: ==> ', actions.value);
    });

    return {
      count,
      isActionButtonsEnabled,
      onIncrease,
      onDecrease,
      actions,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  box-sizing: border-box;
  border: 2px solid #eee;
  padding: 0.5rem;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  align-items: center;
}

.container:not(:last-child) {
  margin-bottom: 1rem;
}

.counter {
  border: 2px solid #eee;
  padding: 0.5rem 2rem;
  border-radius: 0.25rem;
}

button {
  border-radius: 0.375rem;
  padding: 0.5rem;
  border: #eee;
  background-color: blue;
  color: white;
  font-weight: 600;
  font-size: 0.875rem;
  cursor: pointer;
}

button:active {
  box-shadow: 0.25rem 0.25rem 0.5rem 0rem #1a2029;
}

.action button {
  padding: 0.75rem 3rem !important;
}
</style>
