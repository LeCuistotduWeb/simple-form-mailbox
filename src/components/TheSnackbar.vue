<template>
  <v-snackbar
    v-model="snackbar"
    :timeout="timeout"
    :color="color"
    :left="left"
    :right="right"
    :top="top"
    :bottom="bottom"
  >
    {{ text }}
    <v-btn
      flat
      @click="snackbar = false"
    >
      Close
    </v-btn>
  </v-snackbar>
</template>

<script>
export default {
    props:{
        text: {
            type: Boolean,
            default: () => ''
        },
        timeout: {
          type: Number,
          default: () => 3000
        },
        left: Boolean,
        right: Boolean,
        top: Boolean,
        bottom: Boolean,
        color: String,
    },
    data() {
        return {
            snackbar: true,
        }
    },
}
</script>