<template>
    <div class="card mb-4 form-container">
      <div class="card-body">
        <form @submit.prevent="submitUser">
          <!-- Form Fields -->
          <div class="form-floating mb-3">
            <input
              v-model="userData.name"
              type="text"
              class="form-control"
              id="name"
              placeholder="Enter name"
            />
            <label for="name">Name</label>
          </div>
  
          <div class="form-floating mb-3">
            <input
              v-model="userData.email"
              type="email"
              class="form-control"
              id="email"
              placeholder="Enter email"
            />
            <label for="email">Email</label>
            <div v-if="duplicateEmailAlert" class="alert alert-danger mt-2">
              {{ duplicateEmailAlert }}
            </div>
          </div>
  
          <div class="form-floating mb-3">
            <input
              v-model="userData.age"
              type="number"
              class="form-control"
              id="age"
              placeholder="Enter age"
            />
            <label for="age">Age</label>
          </div>
  
          <div class="form-floating mb-3">
            <select v-model="userData.gender" class="form-select" id="gender">
              <option disabled value="">Select gender</option>
              <option>Male</option>
              <option>Female</option>
              <option>Other</option>
            </select>
            <label for="gender">Gender</label>
          </div>
  
          <!-- Button Container -->
          <div class="d-flex justify-content-end">
            <button
              type="submit"
              class="btn btn-primary"
              :disabled="duplicateEmailAlert || !isFormValid"
            >
              <i class="bi" :class="selectedUser ? 'bi-pencil-square' : 'bi-person-plus'"></i>
            </button>
  
            <button
              v-if="selectedUser"
              @click="cancelEdit"
              type="button"
              class="btn btn-secondary ms-2"
            >
              <i class="bi bi-x-circle"></i>
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, watch, computed } from "vue";
  
  export default {
    props: {
      selectedUser: Object,
      users: Array,
    },
    setup(props, { emit }) {
      const userData = ref({ name: "", email: "", age: "", gender: "", index: null });
      const duplicateEmailAlert = ref("");
  
      const isFormValid = computed(() => {
        return (
          userData.value.name &&
          userData.value.email &&
          userData.value.age &&
          userData.value.gender
        );
      });
  
      watch(
        () => props.selectedUser,
        (newVal) => {
          if (newVal) {
            userData.value = { ...newVal };
          } else {
            userData.value = { name: "", email: "", age: "", gender: "", index: null };
          }
        },
        { immediate: true }
      );
  
      watch(
        () => userData.value.email,
        (newEmail) => {
          if (newEmail && !props.selectedUser) {
            const duplicate = props.users.some(
              (existingUser) =>
                existingUser.email === newEmail &&
                existingUser.index !== userData.value.index
            );
            duplicateEmailAlert.value = duplicate ? "User with this email already exists." : "";
          } else {
            duplicateEmailAlert.value = "";
          }
        }
      );
  
      function submitUser() {
        if (isFormValid.value && !duplicateEmailAlert.value) {
          if (userData.value.index !== null) {
            emit("update-user", { ...userData.value });
          } else {
            emit("add-user", {
              name: userData.value.name,
              email: userData.value.email,
              age: userData.value.age,
              gender: userData.value.gender,
            });
          }
          clearForm();
        }
      }
  
      function cancelEdit() {
        emit("update-user", null);
        clearForm();
      }
  
      function clearForm() {
        userData.value = { name: "", email: "", age: "", gender: "", index: null };
        duplicateEmailAlert.value = "";
      }
  
      return { userData, duplicateEmailAlert, isFormValid, submitUser, cancelEdit };
    },
  };
  </script>
  
  <style scoped>
  .form-container {
    max-width: 50%; /* Set form width to half of the container */
    margin: 0 left; /* Center the form horizontally */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Box shadow */
    border-radius: 8px; /* Rounded corners */
  }
  </style>
  