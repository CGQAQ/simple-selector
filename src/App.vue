<script setup>
import SimpleSelectorVue from "./SimpleSelector.vue";

const MockData = {
  title: "root",
  id: 0,
  children: [
    {
      title: "child1",
      id: 1,
      children: [
        {
          title: "child1.1",
          id: 2,
        },
        {
          title: "child1.2",
          id: 3,
          children: [
            {
              title: "child1.2.1",
              id: 4,
            },
            {
              title: "child1.2.2",
              id: 5,
            },
          ],
        },
      ],
    },
    {
      title: "child2",
      id: 6,
      children: [
        {
          title: "child2.1",
          id: 7,
        },
        {
          title: "child2.2",
          id: 8,
        },
      ],
    },
  ],
};
</script>

<template>
  <div>
    <SimpleSelectorVue :data="MockData" />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
