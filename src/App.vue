<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <EmployeeForm @add:employee="addEmployee" />
    <!-- passing employees props to EmployeeTable -->
    <EmployeeTable :employees="employees" @delete:employee="deleteEmployee" />
  </div>
</template>

<script>
import EmployeeTable from "./components/EmployeeTable";
import EmployeeForm from "./components/EmployeeForm";

export default {
  name: "App",
  components: {
    EmployeeTable,
    EmployeeForm
  },
  // data is analogous to React component state
  data() {
    return {
      employees: [
        {
          id: 1,
          name: "Richard Hendricks",
          email: "richard@piedpiper.com"
        },
        {
          id: 2,
          name: "Bertram Gilfoyle",
          email: "gilfoyle@piedpiper.com"
        },
        {
          id: 3,
          name: "Dinesh Chugtai",
          email: "dinesh@piedpiper.com"
        }
      ]
    };
  },
  methods: {
    addEmployee(employee) {
      const lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };
      this.employees = [...this.employees, newEmployee];
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(employee => employee.id !== id);
    }
  }
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
