<template>
  <div>
    <h1>Vue ^3.3.11</h1>
    <label for="counter">Counter</label>
    <input id="counter" type="number" v-model="counter" />
    <div v-for="n in items" :key="n">Item {{ n }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      counter: 5,
    };
  },
  computed: {
    items() {
      if (!this.counter) return [];
      return Array.from({ length: this.counter }, (_, i) => i + 1);
    },
  },
};
</script>
