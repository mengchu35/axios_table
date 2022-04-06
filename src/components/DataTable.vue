<template>
<div>
  <v-data-table :headers="headers" :items="this.$store.state.employees" class="elevation-1">
    <template v-slot:[`item.edit_data`]="{ item }">
      <v-icon
        @click="
          openEditModal=true;
          editDataID=item.id;
          editName=item.name;
          editPhone=item.phone;
          editProfession=item.profession;
        "
      >
        mdi-square-edit-outline
      </v-icon>
    </template>
    <template v-slot:[`item.del_data`]="{ item }">
      <v-icon @click="deleteData(item.id)">mdi-delete</v-icon>
    </template>
  </v-data-table>

  <edit-modal
    :openModal="openEditModal"
    :id="editDataID"
    :name="editName"
    :phone="editPhone"
    :profession="editProfession"
    @closeModal="openEditModal=false"
  >
  </edit-modal>
</div>
</template>

<script>
import DataService from '@/DataService.js'
import EditModal from './EditModal'

export default {
  data () {
    return {
      editDataID: null,
      editName: null,
      editPhone: null,
      editProfession: null,
      openEditModal: false,
      headers: [
        { text: 'ID', value: 'id', align: 'start', sortable: true },
        { text: 'Name', value: 'name' },
        { text: 'Phone', value: 'phone', sortable: false },
        { text: 'Profession', value: 'profession' },
        { text: 'Edit', value: 'edit_data' },
        { text: 'Delete', value: 'del_data' }
      ],
    }
  },
  components: {
    "edit-modal": EditModal
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
  methods: {
    async deleteData(employeeID) {
      try {
        await DataService.deleteData(employeeID)
          .then(response => {
            console.log(response.data);
            location.reload();
          })
      } catch (err) {
        console.log('There was an error:' + err);
      }
    },
  },
}
</script>