<template>
  <div class="container">
    <h1>Users</h1>
    <div class="user-list">
      <div class="user-card" v-for="user in users" :key="user.id">
        <h2>{{ user.name }}</h2>
        <p>{{ user.email }}</p>
        <router-link :to="{ name: 'UserPosts', params: { userId: user.id }}">
          View Posts
        </router-link>
      </div>
    </div>
    <h2>Posts</h2>
    <div class="post-list">
      <div class="post-card" v-for="post in posts" :key="post.id">
        <router-link :to="{ name: 'Post', params: { id: post.id }}">
          <h2>{{ post.title }}</h2>
          <p>{{ post.body.substring(0, 100) }}...</p>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      posts: [],
    };
  },
  mounted() {
    this.fetchUsers();
    this.fetchPosts();
  },
  methods: {
    async fetchUsers() {
      const response = await fetch("https://jsonplaceholder.typicode.com/users");
      this.users = await response.json();
    },
    async fetchPosts() {
      const response = await fetch("https://jsonplaceholder.typicode.com/posts");
      this.posts = await response.json();
    },
  },
};
</script>

<style>
body {
  background-color: #121212;
  color: #e0e0e0;
  font-family: 'Roboto', sans-serif;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #ffffff;
  font-size: 36px;
  margin-bottom: 30px;
}

.user-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 40px;
}

.user-card {
  background-color: #1f1f1f;
  padding: 20px;
  border-radius: 8px;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
}

.user-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.6);
}

.user-card h2 {
  font-size: 22px;
  margin-bottom: 10px;
  color: #bb86fc;
}

.user-card p {
  color: #b0b0b0;
  font-size: 16px;
  line-height: 1.5;
}

.user-card a {
  display: inline-block;
  margin-top: 10px;
  padding: 10px 15px;
  color: #03dac6;
  border: 1px solid #03dac6;
  border-radius: 4px;
  text-decoration: none;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.user-card a:hover {
  background-color: #03dac6;
  color: #121212;
}

.post-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.post-card {
  background-color: #1f1f1f;
  padding: 20px;
  border-radius: 8px;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
}

.post-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.6);
}

.post-card h2 {
  font-size: 22px;
  margin-bottom: 15px;
  color: #bb86fc;
}

.post-card p {
  color: #b0b0b0;
  font-size: 16px;
  line-height: 1.5;
}

a {
  text-decoration: none;
  color: inherit;
}

a:hover {
  color: #03dac6;
}
</style>
