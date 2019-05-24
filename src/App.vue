<template>
  <v-app :dark="dark">
      <TheNavbar/>
    <v-content>
      <v-container>
        <v-layout class="my-3" row justify-space-between>
        <TheSnackbar v-if="error" :text="error" :color="'error'"/>
          <v-flex xs9 sm10 md8 lg8 offset-md1>
            <v-layout row wrap align-center>
              <v-btn @click="loadMails()" color="success" class="ml-0">Boite de reception</v-btn>
              <v-btn @click="loadSpams()" color="error">Spam</v-btn>
              <v-text-field v-model="search" append-icon="search" label="Search" single-line hide-details box />
            </v-layout>
          </v-flex>
        </v-layout>
        <v-layout align-center justify-center>
          <v-flex xs12 md10 center>
            <v-data-table
              :headers="headers"
              :items="mails"
              :pagination.sync="pagination"
              :loading="loading"
              :search="search"
              class="elevation-1"
            >
              <template v-slot:items="props">
                <td>{{ props.item.created_at | formatDate }}</td>
                <td>{{ props.item.data.contact.name }}</td>
                <td>{{ props.item.data.contact.email }}</td>
                <td>{{ props.item.data.contact.message }}</td>
              </template>
            </v-data-table>
            <p>Dernière actualisation: {{ latestUpload }}</p>
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
import TheSnackbar from './components/TheSnackbar'

export default {
  name: 'App',
  components: {
    TheNavbar,
    TheSnackbar,
  },
  data () {
    return {
      mails: [],
      headers: [
        { text: 'Date réception', value: 'created_at', sortable: false },
        { text: 'name', align: 'left', value: 'data.contact.name', sortable: false},
        { text: 'email', value: 'data.contact.email', sortable: false },
        { text: 'message', value: 'data.contact.message', sortable: false },
      ],
      pagination: {'sortBy': 'create_at', 'descending': true},
      loading: false,
      dark: false,
      search: '',
      error: null,
      latestUpload: '',
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
        this.latestUpload = new Date()
        return this.mails = response.data
      }).catch(() => {
        this.mails = []
        this.error = 'service indisponible'
      })
    },
    loadSpams() {
      this.loading = true
      axios.get(SIMPLEFORM_URL + '&spam=true')
      .then(response => {
        this.loading = false
        this.latestUpload = new Date()
        return this.mails = response.data
      }).catch(() => {
        this.mails = []
        this.error = 'service indisponible'
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

const SIMPLEFORM_URL = 'https://getsimpleform.com/messages.json?api_token=' + process.env.VUE_APP_SIMPLEFORM_TOKEN
</script>