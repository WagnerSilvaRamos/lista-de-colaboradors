<template>
    <div id="employee-table">
        <p v-if="employees.length < 1" class="empty-table">
    No employees
  </p>
       <table class="table" v-else>
           <thead>
               <tr>
                   <th>Employee Name</th>
                   <th> Employee Email</th>
               </tr>
           </thead>
          <tbody>
        <tr v-for="employee in employees" :key="employee.id">
         <td v-if="editing === employee.id">
    <input type="text" v-model="employee.name" class="form-control" />
  </td>
  <td v-else>{{employee.name}}</td>
  <td v-if="editing === employee.id">
    <input type="text" v-model="employee.email" class="form-control"  />
  </td>
  <td v-else>{{employee.email}}</td>
  <td v-if="editing === employee.id">
    <button @click="editEmployee(employee)" class="btn btn-success">Save</button>
    <button class="muted-button btn btn-danger" @click="editing = null" >Cancel</button>
  </td>
  <td v-else>
    <button @click="editMode(employee.id)" class="btn btn-info">Edit</button>
    <button @click="$emit('delete:employee', employee.id)" class="btn btn-danger">Delete</button>
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
        employees: Array
    },
    data() {
        return {
            editing: null,
        }
    },
   methods: {
  editMode(id) {
    this.editing = id
  },

  editEmployee(employee) {
    if (employee.name === '' || employee.email === '') return
    this.$emit('edit:employee', employee.id, employee)
    this.editing = null
  }
}
}
</script>

<style scoped>
.btn-info, .btn-info, .btn-success{ margin-right: 10px;}
td{line-height: 40px;}

</style>