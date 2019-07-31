<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" />

    <employee-table v-bind:employees="employees" 
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee" />

  </div>
</template>

<script>

import EmployeeTable from "./components/EmployeeTable.vue";
import EmployeeForm from "./components/EmployeeForm.vue"

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm    
  },
  data() {
    return {
      employees: []
    }
  },
  mounted() {
    this.getEmployees();
  },
  methods: {
    /// GET
    async getEmployees() {
      try {
         const response = await fetch("https://jsonplaceholder.typicode.com/users");
         const data = await response.json();

         this.employees = data;

      } catch (error) {
        console.log(error);
        
      }
    },

    /// POST
    async addEmployee(employee) {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(employee)
        });

        const data = await response.json();
        this.employees = [...this.employees, data];


      } catch (error) {
        console.log(error);
      }
    },

    /// DELETE
    deleteEmployee(id) {
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
        method: "DELETE"
      })
      .then(res => {
        if (res.status === 200) {
          this.employees = this.employees.filter(emp => emp.id != id);
          console.log("deleted");
          
        }
      })
      .catch(error => console.log(error));
    },

    /// PUT
    async editEmployee(id, updatedEmployee) {
      const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(updatedEmployee)
      });

      const data = await response.json();

      this.employees = this.employees.map(emp => {
        return (emp.id === id) ? data : emp;
      });

    }
  }
}
</script>

<style>

</style>