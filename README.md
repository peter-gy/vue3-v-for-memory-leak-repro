# Vue3 Memory Leak When Using `v-for`

Click [here](https://play.vuejs.org/#eNqNU9tu00AQ/ZXBT4lIvEIBHowbtVR9KA+AgDeMkGNPkm33pt11mijyvzO7thM3XIRk2XPbOWfOrI/JjTHprsEkS3KP0ojS47JQAHnNd9EgU5QrFLDW9qpIKt0oj7ZIlredlbOYHmq5Mo0HXo9LwR8MUkA1chX93VzqGsWzGjZ0IGAq6NAUcHqImKOK7BEPIUbY9xSC4xEUtG3OBqqDlbPRKOS6ynLjyca90dZDjeuyER6O4VBd+nIy7WwAi76xavAAen4ZvJl1ofZd+LbRq7SkYbHOhvrI9NyMAmuYvPBb7tK+0XRA+P4jNhph3lhbHtK11XJyBIFq47cZjM8SKkx+zoBP4WoJHF7Cq2nfpOMT3oFfzvqJk1nCZRh5LkuTPjitaM+RXNEnSNcT+yKhixD8Itl6b1zGWFUrOkar4jubKvRMGcmuqYxZosQlzmstrxfp6/Qtae/8OJyik/OV1U8OLTUpkl7BCMMouEM7t6hqtOEC/B/sxbEx9EXqN/ioEAlEonhXabXmmwtJwj65QPvJeK7Vc2lKIfTThxjztsHTLNUWq8c/xB/cvpvps8XIbDS/L+0GfZe++/oR92SfkvRnNKJfw1+SX9Bp0QSOXdn7RtVEe1QX2d7HDXO1+ebu9h6VG4YKRM+3OO7j9h+jn+ku0sVJxfYXvcFk1A==) to open the Vue3 app in Vue SFC Playground.

## Screencast

https://github.com/peter-gy/vue3-v-for-memory-leak-repro/assets/40776291/0dfc6b7e-56e3-4266-8737-aeee32cf4945

## Synopsis

Consider this simple Vue app. It has a counter and a list of items. The list of items is generated using `v-for` and `Array.from`. When the counter is changed, the list of items is updated.

When the counter is changed, the list of items is updated. However, the old HTML nodes used to render the initial list of items become detached elements and keep lingering in memory, causing a memory leak.

```vue
<template>
  <div>
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
```

## Reproduction Steps

You can start the Vue3 app by running the following commands:

```bash
cd v-for-vue3 && npm i && npm run dev
```

Using MS Edge makes it easier to investigate the detached elements, so it is recommended to use it for reproduction.
Learn more about how to use the Edge DevTools to investigate memory leaks [here](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/memory-problems/dom-leaks).

1. Open the app in MS Edge
2. Open the DevTools and navigate to the "Detached Elements" tab
3. Refresh the element state and force an initial garbage collection by clicking the trash icon
4. Set the counter to 0
5. Refresh the element state and force a garbage collection by clicking the trash icon again
6. See that the detached elements are still there

You can verify that this was not the case in Vue2. You can start the very same app in Vue2 by running the following commands:

```bash
cd v-for-vue2 && npm i && npm run dev
```
