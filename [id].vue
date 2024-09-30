<!-- pages/update/[id].vue -->
<template>
    <div>
      <h1>Update User</h1>
      <p>User Update for id number {{ id }}</p>
      <form v-if="user" @submit.prevent="updateUser">
        <div>
          <label for="name">Name:</label>
          <input id="name" v-model="user.name" required>
        </div>
        <div>
          <label for="address">Address:</label>
          <input id="address" v-model="user.address" required>
        </div>
        <div>
          <label for="country">Country:</label>
          <input id="country" v-model="user.country" required>
        </div>
        <button type="submit">Update User</button>
      </form>
      <p v-else-if="error">{{ error }}</p>
      <p v-else>Loading...</p>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useRoute, useRouter } from 'vue-router';
  
  const route = useRoute();
  const router = useRouter();
  const id = route.params.id;
  
  const user = ref(null);
  const error = ref(null);
  
  const fetchUser = async () => {
    try {
      const response = await fetch(`http://localhost:8081/updateuser/${id}`);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      user.value = await response.json();
    } catch (e) {
      console.error('There was a problem fetching the user: ' + e.message);
      error.value = 'Failed to load user data. Please try again later.';
    }
  };
  
  const updateUser = async () => {
    try {
      const response = await fetch(`http://localhost:8081/updateuser/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(user.value),
      });
  
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
  
      // Redirect 
      router.push('/components/UserTable');
    } catch (e) {
      console.error('There was a problem updating the user: ' + e.message);
      error.value = 'Failed to update user. Please try again later.';
    }
  };
  
  onMounted(fetchUser);
  </script>