<template>
  <div id="employee-form">
    <!-- analogous to e.preventDefault() in React event handler -->
    <form @submit.prevent="handleSubmit">
      <label for="name">Employee name</label>
      <input
        ref="first"
        type="text"
        id="name"
        v-model="employee.name"
        :class="{'has-error': submitting && isNameInvalid}"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label for="email">Employee email</label>
      <input
        type="email"
        id="email"
        v-model="employee.email"
        :class="{'has-error': submitting && isEmailInvalid}"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "EmployeeForm",
  // data is analogous to React component state
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.clearStatus();
      this.submitting = true;
      if (this.isNameInvalid || this.isEmailInvalid) {
        this.error = true;
        this.success = false;
        return;
      }
      this.$emit("add:employee", this.employee);
      this.employee = {
        name: "",
        email: ""
      };
      this.submitting = false;
      this.success = true;
      this.error = false;
      // bring focus to first input
      this.$refs.first.focus();
    },
    clearStatus() {
      this.success = false;
      this.error = false;
      this.submitting = false;
    }
  },
  // the function is called every time some data changes. We can access the return value like any other component state anytime.
  computed: {
    isNameInvalid() {
      return this.employee.name === "";
    },
    isEmailInvalid() {
      return this.employee.email === "";
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>