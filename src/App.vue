<script setup lang="ts">
import axios from 'axios';
import { ref } from 'vue';

const formData = ref({
  user: '',
  email: '',
  text: ''
});

const isLoading = ref(false);
const sendPostRequest = async () => {
  isLoading.value = true;

  try {
    const response = await axios.post(import.meta.env.VITE_API_URL, JSON.stringify(formData.value), {
      headers: {
        "Content-Type": "application/json"
      }
    });
    console.log('Response:', response.data);
    formData.value = {
      user: '',
      email: '',
      text: ''
    };
  } catch (error) {
    console.error('Error:', error);
  } finally {
    isLoading.value = false
  }
}
</script>

<template>
  <main class="container">
    <form @submit.prevent="sendPostRequest">
      <div>
        <label for="user">User:</label>
        <input id="user" v-model="formData.user" type="text" required />
      </div>
      <div>
        <label for="email">Email:</label>
        <input id="email" v-model="formData.email" type="email" required />
      </div>
      <div>
        <label for="text">Text:</label>
        <textarea id="text" v-model="formData.text" required></textarea>
      </div>
      <button :aria-busy="isLoading" :disabled="isLoading" type="submit" >Send</button>
    </form>
  </main>
</template>
