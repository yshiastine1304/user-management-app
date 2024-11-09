<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">User Management App</h1>
    <UserForm
      @add-user="addUser"
      @update-user="updateUser"
      :selectedUser="selectedUser"
      :users="users"
    />
    <UserList :users="users" @delete-user="deleteUser" @edit-user="editUser" />
  </div>
</template>

<script>
import UserForm from './UserForm.vue'
import UserList from './UserList.vue'

export default {
  components: { UserForm, UserList },
  data() {
    return {
      users: [], // Array to store user data
      selectedUser: null, // Track the user to be edited
    }
  },
  methods: {
    addUser(user) {
      this.users.push(user);
    },
    deleteUser(index) {
      this.users.splice(index, 1);
    },
    editUser(user, index) {
      this.selectedUser = { ...user, index }; // Add the index to know which user to update
    },
    updateUser(updatedUser) {
      if (updatedUser) {
        this.users[updatedUser.index] = updatedUser;
      }
      this.selectedUser = null;
    },
  },
}
</script>
