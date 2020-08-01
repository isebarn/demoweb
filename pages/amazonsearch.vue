<template>
  <v-container>
    <v-row>
      <v-text-field
        id="id"
        v-model="search"
        name="name"
        label="search"
      />
      <v-btn color="success" @click="search_get">
        Search
      </v-btn>
    </v-row>
    <v-row>
      <v-col v-if="loading_amazon" cols="6">
        Amazon
        <v-progress-circular
          indeterminate
          color="primary"
        />
      </v-col>
    </v-row>
    <v-row>
      <v-data-table
        :headers="headers"
        :items="items"
        :items-per-page="-1"
      >
        <template v-slot:item.src="{ item }">
          <img
            :src="item.src"
            style="width: 100px; height: 100px"
            hide-actions
            @click="openURL(item)"
          >
        </template>
      </v-data-table>
    </v-row>
  </v-container>
</template>

<script>

export default {
  data () {
    return {
      search: '',
      items: [],
      loading_amazon: false,
      headers: [
        {
          text: 'product',
          value: 'src'
        },
        {
          text: 'name',
          value: 'title'
        },
        {
          text: 'price',
          value: 'price'
        },
        {
          text: 'source',
          value: 'source'
        }
      ]
    }
  },

  methods: {
    search_get () {
      const self = this
      self.loading_amazon = true
      this.$api.AmazonSearch.get({ params: { search: this.search } })
        .then(function (response) {
          self.items.push(...response.data)
        })
        .catch(function (error) {
          console.log(error)
        })
        .then(function () {
          self.loading_amazon = false
        })
    },

    openURL (item) {
      window.open(item.url)
    }
  }
}
</script>
