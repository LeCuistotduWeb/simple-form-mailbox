<template>
  <v-toolbar color="red lighten-2" dark app>
      <v-toolbar-title>Lecuistotduweb</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-switch v-model="isDark" :label="isDark ? 'DarkMode': 'LightMode'"/>
      </v-toolbar-items>
    </v-toolbar>
</template>

<script>
  export default {
    props: {
      isDark: {
        type: Boolean,
        default: false
      },
    },
    data() {
      return {
      }
    },
    watch: {
        isDark(){
            this.$emit('isDark')
        }
    },
  }

</script>

<style>
</style>
