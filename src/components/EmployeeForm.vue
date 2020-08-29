<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit" class="container">
      <div class="form-group col-md-6">
        <label for="employeename">Employee name</label>
        <input 
          ref="first"
          type="text" 
          class="form-control"
          :class="{'has-error': submitting && invalidName }" 
          id="employeename" 
          v-model="employee.name"
          @focus='clearStatus'
          @keypress="clearStatus"
        />
      </div>
      <div class="form-group col-md-6">
        <label for="employeeemail">Employee Email</label>
        <input 
          type="text" 
          class="form-control"
          :class="{ 'has-error': submitting && invalidEmail }" 
          id="employeeemail"
          @focus='clearStatus' 
          v-model="employee.email"
        />
      </div>
        <p v-if="error && submitting" class="error-message">
          ❗Please fill out all required fields
        </p>
        <p v-if="success" class="success-message">✅ Employee successfully added</p>
        <button class="btn btn-success btn-md lighten-1 ml-3">Add Employee</button>     
    </form>
  </div>
</template>

<script>
  export default {
    name: 'employee-form',
    data() {
      return {
        submitting: false,
        error: false,
        success: false,
        employee: {
          name: '',
          email: '',
        },
      }
    },
    methods: {
        handleSubmit: function() {
            this.submitting = true
            this.clearStatus()
            
            if(this.invalidName || this.invalidEmail) {
              this.error = true
              return
            }

            // eslint-disable-next-line vue/custom-event-name-casing
            this.$emit('add:employee', this.employee)
            this.$refs.first.focus()

            this.employee = {
              name: "",
              email: "",
            }
            this.error = false
            this.success = true
            this.submitting = false
        },

        clearStatus() {
          this.success = false
          this.error = false
        }
    },
    computed: {
        invalidName() {
            return this.employee.name === ''
        },

        invalidEmail() {
            return this.employee.email === ''
        },
    },
  }
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
  }
  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>