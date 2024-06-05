<template>
    <div class="github-profile-viewer">
        <h1 class="app.title"> Github User Profile</h1>
        <div class="input-container">
            <input v-model="username" placeholder="Enter a Github username" />
            <button @click="getUserProfile">Search</button>
        </div>

       <div v-if="userProfile" class="user-profile">
        <img :src="userProfile.avatar_url" alt="userProfile.login">
       </div>

       <div class="user-details">
                <h2>{{ userProfile.name }}</h2>
                <p>{{ userProfile.bio }}</p>
                <p>{{ userProfile.location }}</p>
                <p>{{ userProfile.blog }}</p>
                <p>{{ twitter_username }}</p>
                <p>{{ userProfile.followers }}</p>
                <p>{{ userProfile.following }}</p>
                <p>{{ userProfile.public_repos }}</p>
                <p>{{ userProfile.created_at }}</p>
                <p>{{ userProfile.updated_at }}</p>

       </div>
    </div>
</template>


<script setup>
import { ref } from 'vue'

const username = ref('')
const userProfile = ref('')
const error = ref('')


const getUserProfile = async () => {
    try {
       const response =await fetch(`https://api.github.com/users/${username.value}`)

       const data = await response.json()
       

         if (response.ok) {
              userProfile.value = data
              error.value = ''
         } else {
              userProfile.value = ''
              error.value = `error: ${data.message}`
         }

    } catch (err) {
        console.error('Failed to get user profile: ', err)
        error.value = 'Failed to get user profile.'
    }
}
</script>

<style scoped>
.github-profile-viewer {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.app-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

.input-container {
  margin-bottom: 20px;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
}

.user-profile {
  margin-top: 20px;
}

h2 {
  font-size: 20px;
  margin-bottom: 10px;
  color: #333;
}

img {
  width: 150px;
  border-radius: 50%;
  margin-bottom: 20px;
}

.user-details {
  text-align: left;
}

p {
  font-size: 16px;
  margin-bottom: 10px;
}

.error-message {
  color: #e74c3c;
  margin-top: 20px;
}
</style>