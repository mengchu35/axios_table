<template>
<div>
  <v-data-table :headers="headers" :items="employees" class="elevation-1"></v-data-table>
</div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      headers: [
        { text: 'ID', value: 'id', align: 'start', sortable: true },
        { text: 'Name', value: 'name' },
        { text: 'Phone', value: 'phone', sortable: false },
        { text: 'Profession', value: 'profession' }
      ],
      employees: []
    }
  },
  created() {
    axios
      .get('http://localhost:3000/employees')
      .then(response => {
        this.employees = response.data;
        console.log(response.data);
      })
      .catch(error => {
        console.log('There was an error:' + error.response);
      })
  },
}
</script>