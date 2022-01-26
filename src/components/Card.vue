<template>
  <div
    :id="id"
    class="card"
    :draggable="draggable"
    @dragstart="dragStart"
    @dragover.prevent
    @drop.prevent="drop"
    @mousedown="mousedown"
  >
    <slot />
  </div>
</template>

<script>
export default {
  props: ["id", "draggable"],
  data() {
    return {
      dropped: false,
    };
  },
  methods: {
    mousedown() {
      this.dropped = false;
    },
    /**начало перетаскивания элемента */
    dragStart(e) {
      const target = e.target;
      e.dataTransfer.setData("card_id", target.id);
      setTimeout(() => {
        if (this.dropped) {
          target.style.display = "none";
        }
      }, 0);
    },
    drop(e) {
      console.info(e);
      const card_id = e.dataTransfer.getData("card_id");
      this.$emit("onAccept", {
        id: this.id,
        appendId: Number.parseInt(card_id),
      });
      this.dropped = true;
    },
  },
};
</script>