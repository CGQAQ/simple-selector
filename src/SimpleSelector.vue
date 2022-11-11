<script setup>
import { ref } from "vue";
import SimpleSelectorLayer from "./SimpleSelectorLayer.vue";

const { data } = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

const data$ = ref(handleTree(data));

function handleTree(data) {
  const result = [];

  const join = (prefix, current) => {
    if (prefix.length === 0) {
      return "" + current;
    } else {
      return prefix + "-" + current;
    }
  };
  const traverse = (node, path = "") => {
    if (!Array.isArray(node.children) || node.children.length === 0) {
      const leaf = { ...node, path: join(path, node.id), checked: false };
      result.push(leaf);
      return leaf;
    }

    const n = {
      ...node,
      path: join(path, node.id),
      children: node.children.map((child) =>
        traverse(child, join(path, node.id))
      ),
      checked: false,
    };

    result.push(n);
    return n;
  };

  traverse(data);

  return result;
}

const result$ = ref("");
</script>

<template>
  <div>
    <SimpleSelectorLayer :root="[data$[data$.length - 1]]" :all="data$" />
    <button
      @click="
        () =>
          (result$ = data$
            .filter((it) => !it.children && it.checked)
            .map((it) => it.title)
            .join(','))
      "
    >
      提交
    </button>
    已选中：{{ result$ }}
  </div>
</template>
