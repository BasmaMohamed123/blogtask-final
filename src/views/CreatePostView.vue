<template>
    <div class="create-post-container">
      <h2 class="header">Create a New Post</h2>
      <form @submit.prevent="submitPost" >
        <input
          v-model="newPost.title"
          type="text"
          placeholder="Title"
          required
          class="input-title"
        />
        <textarea
          v-model="newPost.content"
          placeholder="Content"
          required
          class="textarea-content"
        ></textarea>
        <button type="submit" class="submit-button">Submit Post</button>
      </form>
      <nav class="navigation">
        <router-link to="/" class="nav-link">Home</router-link>
        <router-link to="/blog-posts" class="nav-link">View Blog Posts</router-link>
      
      </nav>
    </div>
  </template>



  <script lang="ts">
  import { defineComponent } from 'vue';
  import axios from 'axios';
  
  export default defineComponent({
    name: 'CreatePost',
    data() {
      return {
        newPost: {
          title: '',
          content: ''
        }
      };
    },
    methods: {
    async submitPost() {
      const authToken = localStorage.getItem('authToken');
      if (!authToken) {
        alert('You must be logged in to create a post.');
        return;
      }

      const url = 'https://interns-blog.nafistech.com/api/posts'; // Replace with your API endpoint

      try {
        const response = await axios.post(
          url,
          {
            title: this.newPost.title,
            content: this.newPost.content,
          },
          {
            headers: {
              Authorization: `Bearer ${authToken}`,
              'Content-Type': 'application/json',
            }
          }
        );
        this.$router.push('/posts'); 

        console.log('Post created:', response.data);
        this.newPost.title = '';
        this.newPost.content = '';
        // Optionally, redirect the user or provide feedback
      
      } catch (error) {
        console.error('Error creating post:', error.response ? error.response.data : error.message);
        alert('Failed to create post. Please try again.');
      }
    }
  },
  mounted() {
    console.log('CreatePostView is mounted');
  }

  });
  </script>
  

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&family=Roboto:wght@400;500&display=swap');
.create-post-container {
    padding: 20px;
    background: url('@/assets/background.jpg') no-repeat center center fixed;
    background-size: cover;
    position: relative;
    overflow: hidden;
    width:auto;
}

h2 {
  text-align: center;
  color: #333;
  margin-bottom: 40px;
  font-size: 2.8em;
}

form {
  display: flex;
  flex-direction: column;
}

.input-title {
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1.1em;
}

.textarea-content {
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1.1em;
  resize: vertical;
  min-height: 150px;
}

.submit-button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  background-color: #3498db;
  color: white;
  font-size: 1.2em;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s ease;
}

.submit-button:hover {
  background-color: #2980b9;
}
</style>
  