<template>
  <div id="employee-table">
    <p v-if="employees.length == 0">No Employees</p>

    <table class="striped-table" v-else>
      <thead>
        <tr>
          <th>Employee #id</th>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="emp in employees" :key="emp.id">
          <td>{{ emp.id }}</td>

          <td v-if="editing == emp.id">
            <input type="text" v-model="emp.name" />
          </td>
          <td v-else>{{ emp.name }}</td>

          <td v-if="editing == emp.id">
            <input type="email" v-model="emp.email" />
          </td>
          <td v-else>{{ emp.email }}</td>

          <td v-if="editing == emp.id">
            <button class="round-button" @click="editEmployee(emp)">Save</button>
            <button class="round-button" @click="cancelEmployee(emp)">Cancel</button>
          </td>
          <td v-else>
            <button class="round-button" @click="editMode(emp)">Edit</button>
            <button class="round-button" @click="$emit('delete:employee', emp.id);">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null,
      cachedEmployee: null
    };
  },

  methods: {
    editMode(emp) {
      this.cachedEmployee = Object.assign({}, emp);
      this.editing = emp.id;
    },
    editEmployee(emp) {
      if (emp.name === "" || emp.email === ""){
        return; 
      }

      this.$emit("edit:employee", emp.id, emp);
      this.editing = null;
    },
    cancelEmployee(emp) {
      Object.assign(emp, this.cachedEmployee);
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