<template>
  <select v-model="value" @change="changeOption">
    <option disabled value="">Choose from list</option>
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'my-select',
  props: {
    value: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    changeOption(event) {
      this.$emit('on-select', event.target.value)
    },
  },
}
</script>

<style scoped></style>
