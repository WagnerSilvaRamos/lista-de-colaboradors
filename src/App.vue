<template>
  <div
    id="app"
    class="small-container"
  >
    <h1>Manager List Employees</h1>

    <EmployeeForm @add:employee="addEmployee" />
    <EmployeeTable
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>

</template>

<script>
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'

export default {
  name: "app",
  components: {
    EmployeeTable,
    EmployeeForm,
  },
  data() {
    return {
      employees: []
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
          headers: { "Content-type": "application/json; charset=UTF-8" }
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (error) {
        console.error(error)
      }
    },

    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => employee.id === id ? data : employee)
      } catch (error) {
        console.error(error)
      }
    },

    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'DELETE'
        })
        this.employees = this.employees.filter(employee => employee.id !== id)
      } catch (error) {
        console.error(error)
      }
    },
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito+Sans:ital,opsz,wght@0,6..12,200..1000;1,6..12,200..1000&family=Nunito:ital,wght@0,200..1000;1,200..1000&display=swap');
body{ margin: 0; padding: 0; font-family: 'Nunito', sans-serif; font-size: 16px;}


#app {
  font-family: 'Nunito', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
 }

.small-container {
  max-width: 50rem;
  margin: 0 auto;
  width: 100%;
}

main{ margin: 5rem auto; background: #f5f5f5; border-radius: 28px; padding: 3rem 0; max-width: 60rem; width: 100%; position: relative}

@media screen and (max-width: 766px) {

  main, .small-container { width: 100%; max-width: 600px; margin: 0 auto; background-color: transparent}
}

</style>
