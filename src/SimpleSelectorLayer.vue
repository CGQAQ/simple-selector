<script setup>
import { ref } from "vue";

const { root, all } = defineProps({
  root: {
    type: Object,
    required: true,
  },
  all: {
    type: Array,
    required: true,
  },
});

const checkedMap = {
  [false]: { text: "unselected", style: "background: none" },
  [true]: { text: "selected", style: "background: green" },
};

const selected$ = ref(0);

function isLeaf(node) {
  return !node.children;
}

function onSelcted(item) {
  if (isLeaf(item)) {
    item.checked = !item.checked;
    return;
  }

  const children = all.filter(
    (i) => i.path.startsWith(item.path) && item.path !== i.path
  );
  const allLeaf = children.filter(isLeaf);
  const shouldUnCheck = allLeaf.every((i) => i.checked);

  children.forEach((it) => (it.checked = !shouldUnCheck));
  item.checked = !shouldUnCheck;
}
</script>

<template>
  <div class="layer-root">
    <div>
      <div v-for="(item, index) in root" style="display: flex" :key="index">
        <div
          class="checkbox"
          :style="checkedMap[item.checked]?.style"
          @click="
            () => {
              onSelcted(item);
            }
          "
        >
          {{ checkedMap[item.checked]?.text }}
        </div>
        <div
          class="layer-item"
          :style="index === selected$ ? 'background: #bbb' : ''"
          @click="() => (selected$ = index)"
        >
          {{ item.title }}({{ (item.children && item.children.length) || 0
          }}{{ " " }}
          个子节点)
        </div>
      </div>
    </div>

    <div>
      <SimpleSelectorLayer
        v-if="selected$ !== -1 && root[selected$].children?.length > 0"
        :root="root?.[selected$]?.children"
        :all="all"
      />
    </div>
  </div>
</template>

<style>
.layer-root {
  display: flex;
  margin-inline: 0.5rem;
  font-size: x-large;
}

.layer-item {
  cursor: pointer;
}

.checkbox {
  border: 2px royalblue solid;
  cursor: pointer;
  font-size: large;
}
</style>
