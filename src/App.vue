<template>
  <div id="app">
    <div class="container">
      <h1>Edicasoft's test assignment</h1>
      <user-form @add:user="addUser" />
      <user-table
        :users="users"
        @delete:user="deleteUser"
        @edit:user="editUser"
      />
    </div>
  </div>
</template>

<script>
import UserTable from "@/components/UserTable.vue";
import UserForm from "@/components/UserForm.vue";
import axios from "axios";

export default {
  name: "app",
  components: {
    UserTable,
    UserForm
  },
  data() {
    return {
      users: [],
      searchTextValue: ""
    };
  },
  mounted() {
    this.getUsers();
  },
  methods: {
    getUsers() {
      axios
        .get("https://jsonplaceholder.typicode.com/users")
        .then(response => {
          this.users = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    addUser(user) {
      const config = { "Content-type": "application/json; charset=UTF-8" };
      axios
        .post("https://jsonplaceholder.typicode.com/users", user, config)
        .then(response => {
          const data = response.data;
          this.users = [data, ...this.users];
        })
        .catch(error => {
          console.log(error);
        });
    },
    editUser(id, updatedUser) {
      const config = { "Content-type": "application/json; charset=UTF-8" };
      axios
        .put(
          `https://jsonplaceholder.typicode.com/users/${id}`,
          updatedUser,
          config
        )
        .then(response => {
          const data = response.data;
          this.users = this.users.map(user => (user.id === id ? data : user));
        })
        .catch(error => {
          console.log(error);
        });
    },
    deleteUser(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/users/${id}`)
        .then(response => {
          console.log(response);
          this.users = this.users.filter(user => user.id !== id);
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style></style>
