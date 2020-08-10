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
      <v-col v-if="loading" cols="6">
        loading rightmove
        <v-progress-circular
          indeterminate
          color="primary"
        />
      </v-col>
      <v-col v-if="items.length > 0">
        <download-csv
          :data="items"
        >
          <v-btn>
            Download Data
          </v-btn>
          <img src="download_icon.png">
        </download-csv>
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
import Vue from 'vue'
import JsonCSV from 'vue-json-csv'
Vue.component('downloadCsv', JsonCSV)
export default {
  data () {
    return {
      search: '',
      items: [],
      loading: false,
      headers: [
        {
          text: 'product',
          value: 'src'
        },
        {
          text: 'address',
          value: 'address'
        },
        {
          text: 'title',
          value: 'title'
        },
        {
          text: 'price',
          value: 'price'
        },
        {
          text: 'bedrooms',
          value: 'bedrooms'
        },
        {
          text: 'bathrooms',
          value: 'bathrooms'
        },
        {
          text: 'latitude',
          value: 'latitude'
        },
        {
          text: 'longitude',
          value: 'longitude'
        }
      ]
    }
  },

  methods: {
    search_get () {
      const self = this
      self.loading = true
      this.$api.RightMove.get({ params: { search: this.search } })
        .then(function (response) {
          self.items.push(...response.data)
        })
        .catch(function (error) {
          console.log(error)
        })
        .then(function () {
          self.loading = false
        })
    },

    openURL (item) {
      window.open(item.url)
    }
  }
}
</script>
