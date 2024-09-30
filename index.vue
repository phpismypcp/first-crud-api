
<template>
    <div>
        <header>
            <nav>
                <ul>
                    <li><NuxtLink to="/components/UserTable">See table</NuxtLink></li>
                    <li><NuxtLink to="/about">About</NuxtLink></li>

                </ul>
            </nav>
        </header>
      <h1>Welcome to my Vue App</h1>
      <UserTable :users="users" :error="error" :loading="loading" />
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const users = ref([]);
  const error = ref(null);
  const loading = ref(true);
  
 

  </script>