<template>
    <div>
      <div v-if="users.length === 0" class="alert alert-warning text-center">
        No users found. Add a new user.
      </div>
      <div v-else class="user-list">
        <UserCard
          v-for="(user, index) in users"
          :key="index"
          :user="user"
          @delete-user="deleteUser(index)"
          @edit-user="editUser(user, index)"
        />
      </div>
    </div>
  </template>
  
  <script>
  import UserCard from './UserCard.vue'
  
  export default {
    components: { UserCard },
    props: ['users'],
    methods: {
      deleteUser(index) {
        this.$emit('delete-user', index)
      },
      editUser(user, index) {
        this.$emit('edit-user', user, index)
      },
    },
  }
  </script>
  
  <style scoped>
  .user-list {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }
  </style>
  