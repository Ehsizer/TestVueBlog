<template>
  <div class="container">
    <div class="post-info">
      <h1>{{ post.title }}</h1>
      <p>{{ post.body }}</p>
      <hr />
      <h2>Author: {{ user.name }}</h2>
      <p><strong>Email:</strong> {{ user.email }}</p>
    </div>
    <div class="comments-section">
      <h3>Comments</h3>
      <div v-for="comment in comments" :key="comment.id" class="comment-card">
        <p><strong>{{ comment.name }}</strong> ({{ comment.email }})</p>
        <p>{{ comment.body }}</p>
      </div>
      <div class="add-comment">
        <h3>Add a Comment</h3>
        <input v-model="newComment.name" placeholder="Your Name" />
        <input v-model="newComment.email" placeholder="Your Email" />
        <textarea v-model="newComment.body" placeholder="Your Comment"></textarea>
        <button @click="addComment">Submit</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      post: {},
      user: {},
      comments: [],
      newComment: {
        name: '',
        email: '',
        body: ''
      }
    };
  },
  mounted() {
    this.fetchPost();
    this.fetchComments();
  },
  methods: {
    async fetchPost() {
      const postId = this.$route.params.id;
      const postResponse = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`);
      const postData = await postResponse.json();
      this.post = postData;

      const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${postData.userId}`);
      this.user = await userResponse.json();
    },
    async fetchComments() {
      const postId = this.$route.params.id;
      const response = await fetch(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`);
      this.comments = await response.json();
    },
    async addComment() {
      const postId = this.$route.params.id;
      const response = await fetch(`https://jsonplaceholder.typicode.com/comments`, {
        method: 'POST',
        body: JSON.stringify({
          ...this.newComment,
          postId: postId
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      });
      const comment = await response.json();
      this.comments.push(comment);
      this.newComment = { name: '', email: '', body: '' };
    }
  },
};
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  color: #f0f0f0;
  background-color: #1e1e1e;
  border-radius: 8px;
}

.post-info {
  margin-bottom: 40px;
}

.comments-section {
  margin-top: 20px;
}

.comment-card {
  background-color: #2a2a2a;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 4px;
}

.add-comment {
  margin-top: 20px;
  background-color: #333333;
  padding: 15px;
  border-radius: 8px;
}

.add-comment input,
.add-comment textarea {
  width: 98%;
  max-width: 98%;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #444;
  border-radius: 4px;
  background-color: #2a2a2a;
  color: #f0f0f0;
}

.add-comment button {
  padding: 10px 15px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.add-comment button:hover {
  background-color: #45a049;
}

</style>