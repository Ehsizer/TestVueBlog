<template>
  <div v-if="post && user && comments">
    <h1>{{ post.title }}</h1>
    <p>{{ post.body }}</p>

    <h3>Author: {{ user.name }}</h3>
    <p>Email: {{ user.email }}</p>

    <h3>Comments</h3>
    <ul>
      <li v-for="comment in comments" :key="comment.id">
        <strong>{{ comment.name }}</strong>: {{ comment.body }}
      </li>
    </ul>

    <form @submit.prevent="submitComment">
      <input v-model="newComment.name" placeholder="Your name" required />
      <textarea v-model="newComment.body" placeholder="Your comment" required></textarea>
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      post: null,
      user: null,
      comments: [],
      newComment: {
        name: '',
        body: '',
        postId: this.$route.params.id,
      },
    };
  },
  methods: {
    async fetchPost() {
      const postId = this.$route.params.id;
      const postResponse = await axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}`);
      this.post = postResponse.data;

      const userResponse = await axios.get(`https://jsonplaceholder.typicode.com/users/${this.post.userId}`);
      this.user = userResponse.data;

      const commentsResponse = await axios.get(`https://jsonplaceholder.typicode.com/posts/${postId}/comments`);
      this.comments = commentsResponse.data;
    },
    async submitComment() {
      try {
        const response = await axios.post('https://jsonplaceholder.typicode.com/comments', this.newComment);
        this.comments.push(response.data);
        this.newComment.name = '';
        this.newComment.body = '';
        this.$emit('notify', { message: 'Comment added successfully!', type: 'success' });
      } catch (error) {
        this.$emit('notify', { message: 'Failed to add comment.', type: 'error' });
      }
    },
  },
  mounted() {
    this.fetchPost();
  },
};
</script>
