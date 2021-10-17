<template>
  <div>
    <form @submit.prevent>
      <h2>Сreate To-do</h2>
      <!-- Двухсторонее связывание -->
      <my-input v-model="post.title" type="text" placeholder="Name:" />
      <!-- v-bind:value="post.title"
          @input="post.title = $event.target.value" -->
      <my-input v-model="post.body" type="text" placeholder="Discription:" />
      <my-button @click="createPost" style="align-self:end; margin-top: 15px"
        >Create</my-button
      >
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: '',
      },
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: '',
      }
    },
  },
}
</script>

<style>
form {
  display: flex;
  flex-direction: column;
}
</style>
