<template>
<div>
  <v-data-table :headers="headers" :items="this.$store.state.employees" class="elevation-1"></v-data-table>
</div>
</template>

<script>
import DataService from '@/DataService.js'

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
    DataService.getData()
      .then(response => {
        this.$store.state.employees = response.data;
        console.log(response.data);
      })
      .catch(error => {
        console.log('There was an error:' + error.response);
      })
  },
}
</script>