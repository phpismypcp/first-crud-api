
<template>
    <div>
      <h2>Add New User</h2>
      <form @submit.prevent="submitForm">
        <div>
          <label for="name">Name:</label>
          <input id="name" v-model="form.name" required>
        </div>
        <div>
          <label for="address">Address:</label>
          <input id="address" v-model="form.address" required>
        </div>
        <div>
          <label for="country">Country:</label>
          <input id="country" v-model="form.country" required>
        </div>
        <button type="submit">Add User</button>
        <li><NuxtLink to="/components/UserTable">Cancel/Back to Table</NuxtLink></li>
      </form>
      <p v-if="message">{{ message }}</p>
    </div>
  </template>
  
  <script setup>
  import { ref, reactive } from 'vue';
  
  const form = reactive({
    name: '',
    address: '',
    country: ''
  });
  
  const message = ref('');
  
  const emit = defineEmits(['user-added']);
  
  const submitForm = async () => {
    try {
      const response = await fetch('http://localhost:8081/createuser', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(form),
      });
  
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
  
      const result = await response.json();
      message.value = 'User added successfully!';
      emit('user-added', result);
  
      // Reset form
      form.name = '';
      form.address = '';
      form.country = '';
    } catch (error) {
      console.error('Error adding user:', error);
      message.value = 'Failed to add user. Please try again.';
    }
  };
  </script>