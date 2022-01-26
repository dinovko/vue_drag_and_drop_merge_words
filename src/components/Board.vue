<template>
  <div :id="id" class="board">
    <slot />
  </div>
</template>

<script>
export default {
  props: ["id", "array"],
  methods: {},
};
</script>