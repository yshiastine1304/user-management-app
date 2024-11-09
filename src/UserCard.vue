<template>
    <div class="card mb-3 shadow-card">
      <div class="card-body">
        <h5 class="card-title">{{ user.name }}</h5>
        <p class="card-text"><strong>Email:</strong> {{ user.email }}</p>
        <p class="card-text"><strong>Age:</strong> {{ user.age }}</p>
        <p class="card-text"><strong>Gender:</strong> {{ user.gender }}</p>
        <div class="d-flex justify-content-end">
          <button @click="$emit('edit-user')" class="btn btn-secondary me-2">
            <i class="bi bi-pencil-square"></i>
          </button>
          <button @click="$emit('delete-user')" class="btn btn-danger">
            <i class="bi bi-trash"></i>
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['user'],
  }
  </script>
  
  <style scoped>
  .shadow-card {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Box shadow */
    border-radius: 8px; /* Rounded corners */
  }
  </style>
  