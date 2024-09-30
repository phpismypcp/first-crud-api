<!-- UserTable.vue -->
<template>
  <div>
    <header>
      <nav>
        <ul>
          <li><NuxtLink to="/">Home</NuxtLink></li>
          <li><NuxtLink to="/about">About</NuxtLink></li>
        </ul>
      </nav>
    </header>
    <h2>User List</h2>
    <li><NuxtLink to="/components/createuser">Add+</NuxtLink></li>
    <table v-if="users.length">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Address</th>
          <th>Country</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.address }}</td>
          <td>{{ user.country }}</td>
          <td>
            <NuxtLink :to="`/components/${user.id}`" class="btn btn-primary">Update</NuxtLink>
            <button @click="deleteUser(user.id)" class='btn btn-danger ms-2'>Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else-if="error">{{ error }}</p>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const users = ref([]);
const error = ref(null);

const fetchUsers = async () => {
  try {
    const response = await fetch('http://localhost:8081');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    users.value = await response.json();
  } catch (e) {
    console.error('There was a problem with the fetch operation: ' + e.message);
    error.value = 'Failed to load users. Please try again later.';
  }
};

const deleteUser = async (id) => {
  if (!confirm('Are you sure you want to delete this user?')) {
    return;
  }

  try {
    const response = await fetch(`http://localhost:8081/user/${id}`, {
      method: 'DELETE',
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // Remove the deleted user from the local state
    users.value = users.value.filter(user => user.id !== id);
  } catch (e) {
    console.error('There was a problem deleting the user: ' + e.message);
    error.value = 'Failed to delete user. Please try again later.';
  }
};

onMounted(fetchUsers);
</script>