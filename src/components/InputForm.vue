<template>
  <div>
    <v-form>
      <v-container>
        <v-row>
          <v-col cols="12" sm="3">
            <v-text-field label="Name" v-model="name" required></v-text-field>
          </v-col>
          <v-col cols="12" sm="3">
            <v-text-field label="Phone" v-model="phone" required></v-text-field>
          </v-col>
          <v-col cols="12" sm="3">
            <v-text-field label="Profession" v-model="profession" required></v-text-field>
          </v-col>
          <v-col cols="12" sm="3">
          <v-btn class="mt-3" @click="addEmployee">Send</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>

<script>
import DataService from '@/DataService.js'

export default {
  data () {
    return {
      id: 0,
      name: '',
      phone: '',
      profession: ''
    }
  },
  created() {
    DataService.getData()
      .then(response => {
        this.id = response.data[response.data.length - 1].id;
        console.log(this.id);
      })
      .catch(error => {
        console.log('There was an error:' + error.response);
      })
  },
  methods: {
    addEmployee () {
      let employeeData = {
        "id": this.id + 1,
        "name": this.name,
        "phone": this.phone,
        "profession": this.profession
      };
      DataService.addData(employeeData)
        .then(response => {
          console.log(response.data);
          location.reload();
        })
        .catch(error => {
          console.log('There was an error:' + error.response);
        })

      this.name = '';
      this.phone = '';
      this.profession = '';
    }
  },
}
</script>