<template>
  <v-container>
    <v-row>
      <v-combobox
        v-model="ebookcategory"
        :items="ebookcategories"
        label="EBook Category"
        item-text="Value"
        item-value="Id"
        @change="chooseebookcategory"
      />
    </v-row>
    <v-row>
      <v-col v-if="loading_dison" cols="6">
        loading
        <v-progress-circular
          indeterminate
          color="primary"
        />
      </v-col>
    </v-row>
    <v-row>
      <v-data-table
        :headers="headers"
        :items="items_display"
        :items-per-page="-1"
      >
        <template v-slot:item.ASIN="{ item }">
          <v-btn
            hide-actions
            @click="openURL(item)"
          >
            {{ item.ASIN }}
          </v-btn>
        </template>
      </v-data-table>
    </v-row>
  </v-container>
</template>

<script>

export default {

  fetch () {
    const self = this
    self.loading_dison = true
    this.$api.DisonSearch.get()
      .then(function (response) {
        console.log(response.data[0])
        self.items.push(...response.data)
        self.items_display = self.items
      })
      .catch(function (error) {
        console.log(error)
      })
      .then(function () {
        self.loading_dison = false
      })
    this.$api.DisonSearch.ebookcategories()
      .then(function (response) {
        console.log(response.data)
        self.ebookcategories.push(...response.data)
      })
      .catch(function (error) {
        console.log(error)
      })
      .then(function () {
        self.loading_dison = false
      })
  },
  data () {
    return {
      search: '',
      items: [],
      items_display: [],
      ebookcategories: [],
      ebookcategory: null,
      loading_dison: false,
      headers: [
        {
          text: 'ASIN',
          value: 'ASIN'
        },
        {
          text: 'title',
          value: 'Title'
        },
        {
          text: 'author',
          value: 'Author'
        },
        {
          text: 'paperbackISBN',
          value: 'PaperbackISBN'
        }
      ]
    }
  },

  methods: {
    openURL (item) {
      window.open(item.URL)
    },

    chooseebookcategory () {
      this.items_display = this.items.filter(
        x => x.eBookCategory_1 === this.ebookcategory.Id ||
        x.eBookCategory_2 === this.ebookcategory.Id ||
        x.eBookCategory_3 === this.ebookcategory.Id)
    }
  }
}
</script>
