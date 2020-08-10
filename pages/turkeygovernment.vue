<template>
  <v-container>
    <v-row>
      <v-text-field
        id="id"
        v-model="start"
        name="start"
        label="start"
        hint="Input 01.02.2020 for Febuary 1 2020"
      />
    </v-row>
    <v-row>
      <v-text-field
        id="id"
        v-model="end"
        name="end"
        label="end"
        hint="Input 01.03.2020 for March 1 2020"
      />
    </v-row>
    <v-row>
      <v-btn color="success" @click="search_get">
        Go
      </v-btn>
    </v-row>
    <v-row>
      <v-col v-if="loading" cols="6">
        Loading
        <v-progress-circular
          indeterminate
          color="primary"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

export default {
  data () {
    return {
      start: '01.02.2020',
      end: '01.03.2020',
      loading: false
    }
  },

  methods: {
    search_get () {
      const self = this
      self.loading = true
      this.$api.Turkey.generate({ params: { start: this.start, end: this.end } })
        .then(function (response) {
          window.open('https://demoapi.isebarn.com/get_excel')
        })
        .catch(function (error) {
          console.log(error)
        })
        .then(function () {
          self.loading = false
        })
    },

    openURL () {
    }
  }
}
</script>
