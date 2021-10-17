<template>
  <div v-if="posts && posts.length > 0">
    <h3>To-do list</h3>
    <transition-group name="post-list">
      <post-item
        v-for:="post in posts"
        :post="post"
        :key="post.id"
        @remove="$emit('remove', post)"
      />
    </transition-group>
  </div>
  <h2 v-else style="color:brown">You don`t have a to-do</h2>
</template>

<script>
import PostItem from "./PostItem.vue"
export default {
  components: { PostItem },
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
}
</script>

<style scoped>
.post-list-item {
  display: inline-block;
  margin-right: 10px;
}
.post-list-active,
.post-list-active {
  transition: all 1s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.post-list-move {
  transition: transform 0.8s ease;
}
</style>
