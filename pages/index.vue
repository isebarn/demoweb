<template>
  <v-layout row wrap>
    Hi
  </v-layout>
</template>

<script>

export default {
  created () {
    // this.$axios.get('god', { params: { search: this.search.replace(' ', '+') } })
    this.$api.Data.test({ params: { search: 'adolf hitler' } })
      .then(function (response) {
        // handle success
        console.log(response.data)
      })
      .catch(function (error) {
        // handle error
        console.log(error)
      })
      .then(function () {
      })
  }
}
</script>
