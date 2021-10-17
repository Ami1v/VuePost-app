<template>
  <div class="app">
    <h1>Page with to-do</h1>
    <my-input v-model="searchQuery" placeholder="Search..." />
    <div class="app__btns">
      <my-button @click="showDialog">
        Create your to-do
      </my-button>
      <my-select
        :value="selectedSort"
        :options="sortOptions"
        @on-select="onSelect"
      />
    </div>
    <!-- <input type="text" v-model.trim="modificatorValue" /> -->
    <!-- <my-button @click="fetchPosts">Get to-do</my-button> -->
    <my-dialog :show="dialogVisible" @click="closeDialog">
      <post-form @create="createPost" />
    </my-dialog>

    <post-list
      v-if="!isLoading"
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
    />
    <div v-else>Loader...</div>
    <div ref="observer" class="observer"></div>
    <!-- <div class="page__wrapper">
      <div
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': page === pageNumber,
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
  </div>
</template>

<script>
import PostForm from '@/components/PostForm'
import PostList from '@/components/PostList'
import MyButton from './components/UI/MyButton'
import axios from 'axios'
import MySelect from './components/UI/MySelect.vue'

export default {
  components: {
    PostList,
    PostForm,
    MyButton,
    MySelect,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      /*  modificatorValue: '', */
      isLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        { value: 'title', name: 'By name' },
        { value: 'body', name: 'By content' },
      ],
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.closeDialog()
    },
    /* this.title = event.target.value; Двусторнее связывание */
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    closeDialog() {
      this.dialogVisible = false
    },
    onSelect(value) {
      this.selectedSort = value
    },
    /* changePage(pageNumber) {
      this.page = pageNumber
      this.fetchPosts()
    }, */
    async fetchPosts() {
      try {
        this.isLoading = true
        const response = await axios.get(
          'https://jsonplaceholder.typicode.com/posts?_limit=10',
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        )
        this.totalPages = Math.ceil(
          response.headers['x-total-count'] / this.limit
        )
        this.posts = response.data
        this.isLoading = false
      } catch (e) {
        alert('Error')
      }
    },
    async loadMorePosts() {
      try {
        this.isLoading = true
        const response = await axios.get(
          'https://jsonplaceholder.typicode.com/posts?_limit=10',
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        )
        this.totalPages = Math.ceil(
          response.headers['x-total-count'] / this.limit
        )
        this.posts = [...this.posts, ...response.data]
        this.posts = response.data
        this.isLoading = false
      } catch (e) {
        alert('Error')
      }
    },
    mounted() {
      this.fetchPosts()
      console.log(this.$refs.observer)
      const options = {
        rootMargin: '0px',
        threshold: 1.0,
      }
      const callback = function(entries, observer) {
        console.log('down')
      }
      const observer = new IntersectionObserver(callback, options)
      observer.observe(this.observer)
    },
  },
  async created() {
    await this.fetchPosts()
  },
  computed: {
    sortedPosts() {
      if (!this.selectedSort) {
        return this.posts
      } else if (this.selectedSort === 'title') {
        return this.posts.sort((a, b) => a.title.localeCompare(b.title))
      } else if (this.selectedSort === 'body') {
        return this.posts.sort((a, b) => a.body.localeCompare(b.body))
      }
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      )
    },
  },
  watch: {
    /* page() {
      this.fetchPosts()
    }, */
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}
.current-page {
  border: 2px solid teal;
}
.observer {
  height: 30px;
  background: green;
}
</style>
