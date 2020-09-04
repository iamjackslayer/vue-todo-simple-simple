<template>
  <div id="employee-table">
    <p v-if="employees.length === 0" class="empty-table">Employees Kosong</p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
            <!-- v-model combines onChange and value properties of input in React -->
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="email" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button @click="handleSave(employee.id)">Save</button>
            <button @click="handleCancel(employee.id)" class="muted-button">Cancel</button>
          </td>
          <td v-else>
            <button @click="handleEdit(employee)">Edit</button>
            <button @click="handleDelete(employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "EmployeeTable",
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    handleEdit(employee) {
      this.cachedEmployee = Object.assign({}, employee);
      this.editing = employee.id;
    },
    handleDelete(id) {
      this.$emit("delete:employee", id);
    },
    handleSave(id) {
      this.editing = null;
      this.$emit("save:employee", id);
    },
    handleCancel(id) {
      this.employees = this.employees.map(employee =>
        employee.id === id ? Object.assign({}, this.cachedEmployee) : employee
      );
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>