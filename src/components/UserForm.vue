<template>
  <div>
    <button class="btn btn-primary" @click="formShown = !formShown">
      {{
        formShown
          ? "Hide form for adding new user"
          : "Show form for adding new user"
      }}
    </button>
    <div v-show="formShown">
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="exampleInputUsername">Username</label>
          <input
            type="text"
            class="form-control"
            id="exampleInputUsername"
            :class="{ 'has-error': submitting && invalidUsername }"
            v-model="user.username"
            @focus="clearStatus"
            required
          />
        </div>
        <div class="form-group">
          <label for="exampleInputEmail1">Email address</label>
          <input
            type="email"
            class="form-control"
            id="exampleInputEmail1"
            :class="{ 'has-error': submitting && invalidEmail }"
            v-model="user.email"
            @focus="clearStatus"
            required
          />
        </div>

        <div class="form-group">
          <label for="exampleInputStreet">City</label>
          <input
            type="text"
            class="form-control"
            id="exampleInputStreet"
            :class="{ 'has-error': submitting && invalidUsername }"
            v-model="user.address.city"
            @focus="clearStatus"
          />
        </div>

        <div class="form-group">
          <label for="exampleInputStreet">Street</label>
          <input
            type="text"
            class="form-control"
            id="exampleInputStreet"
            :class="{ 'has-error': submitting && invalidUsername }"
            v-model="user.address.street"
            @focus="clearStatus"
          />
        </div>

        <div class="form-group">
          <label for="exampleInputStreet">Suite</label>
          <input
            type="text"
            class="form-control"
            id="exampleInputStreet"
            :class="{ 'has-error': submitting && invalidUsername }"
            v-model="user.address.suite"
            @focus="clearStatus"
          />
        </div>

        <p v-if="error && submitting" class="error-message">
          ❗Please fill out all required fields
        </p>
        <p v-if="success" class="success-message">
          ✅ User successfully added
        </p>

        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "users-form",
  data() {
    return {
      error: false,
      submitting: false,
      success: false,
      formShown: false,
      user: {
        email: "",
        username: "",
        address: {
          street: "",
          city: "",
          suite: ""
        }
      }
    };
  },
  computed: {
    invalidName() {
      return this.user.name === "";
    },
    invalidEmail() {
      return this.user.email === "";
    }
  },
  methods: {
    handleSubmit() {
      this.clearStatus();
      this.submitting = true;
      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }
      this.$emit("add:user", this.user);
      this.user = {
        email: "",
        username: "",
        address: {
          street: "",
          city: "",
          suite: ""
        }
      };
      this.success = true;
      this.error = false;
      this.submitting = false;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    }
  }
};
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
</style>
