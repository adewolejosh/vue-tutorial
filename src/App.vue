<template>
  <div id="app" class="mt-md-4 container">
    <h1>Employees</h1>
    
    <employee-form  @add:employee="addEmployee"/>
    <employee-table :employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee" />
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue';
  import EmployeeForm from '@/components/EmployeeForm.vue';

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm,
    },
    data() {
      return {
        // {
        //   id: 1,
        //   name: 'Richard Hendricks',
        //   email: 'richard@piedpiper.com',
        // },
        // {
        //   id: 2,
        //   name: 'Bertram Gilfoyle',
        //   email: 'gilfoyle@piedpiper.com',
        // },
        // {
        //   id: 3,
        //   name: 'Dinesh Chugtai',
        //   email: 'dinesh@piedpiper.com',
        // },
        employees: [],
      }
    },

    mounted() {
      this.getEmployees()
    },

    methods: {
      async getEmployees() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
        } catch (error) {
          console.error(error)
        }
      },

      async addEmployee(employee) {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = [...this.employees, data]
        } catch (error) {
          console.error(error)
        }
      },
      // addEmployee(employee) {
      //   const lastId = this.employees.length > 0 ? this.employees[this.employees.length - 1].id : 0;
      //   const id = lastId + 1;
      //   const newEmployee = { ...employee, id };

      //   this.employees = [...this.employees, newEmployee]
      // },

      async editEmployee(id, updatedEmployee) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'PUT',
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
        } catch (error) {
          console.error(error)
        }
      },

      // editEmployee(id, updatedEmployee) {
      //   this.employees = this.employees.map(employee =>
      //     employee.id === id ? updatedEmployee : employee
      //   )
      // },

      async deleteEmployee(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: "DELETE"
          });
          this.employees = this.employees.filter(employee => employee.id !== id);
        } catch (error) {
          console.error(error);
        }
      },

      // deleteEmployee(id) {
      //   this.employees = this.employees.filter(
      //     employee => employee.id !== id
      //   )
      // },
    }
}
</script>

<style>
  .small-container {
    max-width: 680px;
  }
</style>