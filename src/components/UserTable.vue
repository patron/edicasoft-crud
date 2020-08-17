<template>
  <div>
    <hr />
    <form class="form-inline form-custom-class">
      <div class="form-group">
        <label for="inputSearch">Search by username</label>
        <div class="form-group col-md-6">
          <input
            type="text"
            id="inputSearch"
            placeholder="type username"
            v-model="searchTerm"
          />
        </div>
      </div>
    </form>

    <p v-if="users.length < 1" class="empty-table">
      No users
    </p>
    <table class="table table-hover" v-else>
      <thead>
        <tr>
          <th>Username</th>
          <th>Email</th>
          <th>City</th>
          <th>Street</th>
          <th>Suite</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr :key="user.id" v-for="user in filterByTerm">
          <td v-if="editing === user.id">
            <input type="text" v-model="user.username" />
          </td>
          <td v-else>{{ user.username }}</td>

          <td v-if="editing === user.id">
            <input type="text" v-model="user.email" />
          </td>

          <td v-else>{{ user.email }}</td>

          <td v-if="editing === user.id">
            <input type="text" v-model="user.address.city" />
          </td>
          <td v-else>{{ user.address.city }}</td>

          <td v-if="editing === user.id">
            <input type="text" v-model="user.address.street" />
          </td>
          <td v-else>{{ user.address.street }}</td>

          <td v-if="editing === user.id">
            <input type="text" v-model="user.address.suite" />
          </td>
          <td v-else>{{ user.address.suite }}</td>

          <td v-if="editing === user.id">
            <button
              class="btn btn-success mr-1 btn-custom"
              @click="editUser(user)"
            >
              Save
            </button>
            <button
              class="btn btn-secondary mr-1 btn-custom"
              @click="cancelEdit(user)"
            >
              Cancel
            </button>
          </td>
          <td v-else>
            <button
              class="btn btn-success mr-1 btn-custom"
              @click="editMode(user)"
            >
              Edit
            </button>
            <button
              @click="$emit('delete:user', user.id)"
              class="btn btn-danger btn-custom"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "user-table",
  props: {
    users: Array
  },
  data() {
    return {
      editing: null,
      searchTerm: ""
    };
  },
  methods: {
    editMode(user) {
      this.cachedUser = Object.assign({}, user);
      this.editing = user.id;
    },
    cancelEdit(user) {
      Object.assign(user, this.cachedUser);
      this.editing = null;
    },
    editUser(user) {
      if (user.name === "" || user.email === "") return;
      this.$emit("edit:user", user.id, user);
      this.editing = null;
    }
  },
  computed: {
    filterByTerm() {
      if (this.searchTerm === "") {
        return this.users;
      }
      return this.users.filter(user => {
        return user.username
          .toLowerCase()
          .includes(this.searchTerm.toLowerCase());
      });
    }
  }
};
</script>

<style scoped>
.btn-custom {
  width: 85px;
}
.form-custom-class {
  padding-bottom: 15px;
}
</style>
