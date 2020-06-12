<template>
  <div>
      <v-toolbar tabs>
          <v-toolbar-title >Fancy quiz app</v-toolbar-title>
          <b-nav-item >Counter: {{ numCorrect }} / {{ numTotal }}</b-nav-item>
      </v-toolbar>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>