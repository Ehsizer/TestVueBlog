<template>
  <div>
    <select v-model="selectedUserId" @change="fetchPosts">
      <option value="">All Users</option>
      <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
    </select>

    <ul>
      <li v-for="post in filteredPosts" :key="post.id">
        <router-link :to="{ name: 'Post', params: { id: post.id }}">{{ post.title }}</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      posts: [],
      users: [],
      selectedUserId: '',
    };
  },
  computed: {
    filteredPosts() {
      return this.selectedUserId
          ? this.posts.filter(post => post.userId === parseInt(this.selectedUserId))
          : this.posts;
    },
  },
  methods: {
    async fetchPosts() {
      const postsResponse = await axios.get('https://jsonplaceholder.typicode.com/posts');
      this.posts = postsResponse.data;
    },
    async fetchUsers() {
      const usersResponse = await axios.get('https://jsonplaceholder.typicode.com/users');
      this.users = usersResponse.data;
    },
  },
  mounted() {
    this.fetchPosts();
    this.fetchUsers();
  },
};
</script>
