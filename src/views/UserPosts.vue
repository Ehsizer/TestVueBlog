<template>
  <div class="container">
    <div class="user-info">
      <h1>{{ user.name }}</h1>
      <p><strong>Email:</strong> {{ user.email }}</p>
    </div>
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
      user: {},
      posts: [],
    };
  },
  mounted() {
    this.fetchUser();
    this.fetchUserPosts();
  },
  methods: {
    async fetchUser() {
      const userId = this.$route.params.userId;
      const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
      this.user = await response.json();
    },
    async fetchUserPosts() {
      const userId = this.$route.params.userId;
      const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`);
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

.user-info {
  margin-bottom: 40px;
  padding: 20px;
  background-color: #1f1f1f;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
}

.user-info h1 {
  color: #bb86fc;
  font-size: 28px;
  margin-bottom: 10px;
}

.user-info p {
  color: #b0b0b0;
  font-size: 16px;
  line-height: 1.5;
}

.user-info a {
  color: #03dac6;
  text-decoration: none;
}

.user-info a:hover {
  text-decoration: underline;
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
