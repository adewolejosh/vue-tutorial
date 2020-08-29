<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table class="table table-bordered" v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" class="form-control" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="text"  class="form-control" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button class="btn btn-success btn-md lighten-1 ml-3" @click="editEmployee(employee)">Save</button>
            <button class="btn btn-danger btn-md lighten-1 ml-3 muted-button" @click="cancelEdit(employee)">Cancel</button>
          </td>
          <td v-else>
            <button class="btn btn-primary btn-md lighten-1 ml-3" @click="editMode(employee)">Edit</button>
            <button class="btn btn-danger btn-md lighten-1 ml-3" @click="deleteMode(employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'employee-table',
    props:{
      employees: Array,
    },

    data() {
      return {
        editing: null,
      }
    },

    methods: {
      
      editMode(employee) {
        this.cachedEmployee = Object.assign({}, employee)
        this.editing = employee.id
      },
      editEmployee(employee) {
          if (employee.name === '' || employee.email === '') return
          // eslint-disable-next-line vue/custom-event-name-casing
          this.$emit('edit:employee', employee.id, employee)
          this.editing = null
      },

      cancelEdit(employee) {
        Object.assign(employee, this.cachedEmployee)
        this.editing = null;
      },

      deleteMode: function(id){
          // eslint-disable-next-line vue/custom-event-name-casing
          this.$emit('delete:employee', id)        
      },

    },
  }
</script>

<style scoped></style>