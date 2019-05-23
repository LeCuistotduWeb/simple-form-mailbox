<template>
  <v-app :dark="dark">
      <TheNavbar/>
    <v-content>
      <v-container>
        <v-layout class="my-3" align-center justify-center>
          <v-flex xs10>
            <v-btn @click="loadMails()" color="success" class="ml-0">Boite de reception</v-btn>
            <v-btn @click="loadSpams()" color="error" class="ml-0">Spam</v-btn>
          </v-flex>
        </v-layout>
        <v-layout align-center justify-center>
          <v-flex xs10 center>
            <v-data-table
              :headers="headers"
              :items="mails"
              :loading="loading"
              class="elevation-1"
            >
              <template v-slot:items="props">
                <td>{{ props.item.created_at | formatDate }}</td>
                <td>{{ props.item.data.contact.name }}</td>
                <td>{{ props.item.data.contact.email }}</td>
                <td>{{ props.item.data.contact.message }}</td>
              </template>
            </v-data-table>
            <v-switch v-model="dark" primary label="Dark" />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import axios from 'axios'
import TheNavbar from './components/TheNavbar'

export default {
  name: 'App',
  components: {
    TheNavbar,
  },
  data () {
    return {
      mails: [],
        headers: [
          { text: 'Date réception', value: 'created_at' },
          { text: 'name', align: 'left', value: 'data.contact.name', sortable: false},
          { text: 'email', value: 'data.contact.email', sortable: false },
          { text: 'message', value: 'data.contact.message', sortable: false },
        ],
        loading: false,
        dark: false,
    }
  },
  created() {
    this.loadMails()
  },
  methods: {
    loadMails() {
      this.loading = true
      axios.get(SIMPLEFORM_URL)
      .then(response => {
        this.loading = false
        return this.mails = response.data
      })
    },
    loadSpams() {
      this.loading = true
      axios.get(SIMPLEFORM_URL + '&spam=true')
      .then(response => {
        this.loading = false
        return this.mails = response.data
      })
    }
  },
  filters: {
    formatDate: function (value) {
      if (!value) return ''
      let newDate = new Date(value)
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }
      return newDate.toLocaleDateString('fr-FR', options)
    }
  }
}
const SIMPLEFORM_URL = 'https://getsimpleform.com/messages.json?api_token=<api_token>'
</script>