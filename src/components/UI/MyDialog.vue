<template>
  <div class="dialog" v-if="show">
    <div @click.stop class="dialog__content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'my-dialog',
  props: {
    show: {
      /* boolean flag */
      type: Boolean,
      default: false,
    },
  },
}
</script>

<style scoped>
.dialog {
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}

.dialog__content {
  margin: auto;
  background: white;
  border-radius: 12px;
  min-height: 60px;
  min-width: 400px;
  padding: 25px;
}
</style>
