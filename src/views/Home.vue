<template>
  <div class="container">
    <div class="row">
      <b-col cols="12" sm="6" xl="4">
        <b-form-input type="text" class="searchInput" v-model="filter" placeholder="Search movie..."></b-form-input>
      </b-col>
    </div>
    <b-table borderless striped hover :items="filteredMovies" :fields="fields" class="my-5">
      <template v-slot:cell(more)="data">
        <b-modal
          :id="`modal-${data.item.id}`"
          centered
          :title="data.item.title"
          ok-only
          header-bg-variant="primary"
          header-text-variant="secondary"
          body-bg-variant="dark"
          body-text-variant="secondary"
          footer-text-variant="secondary"
          footer-class="my-footer"
        >
          <p class="my-4"><small class="small">artist:</small> {{ data.item.artist }}</p>
          <p class="my-4"><small class="small">release date:</small> {{ data.item.released }}</p>
        </b-modal>
        <b-button @click="$bvModal.show(`modal-${data.item.id}`)" variant="primary" class="gold-font">
          More...
        </b-button>
      </template>
    </b-table>
  </div>
</template>

<style lang="scss" src="@/styles/home.scss" scoped></style>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  data() {
    return {
      filter: null,
      topmovies: [],
      fields: [
        { key: 'title', label: 'Movie Title', tdClass: 'title-col' },
        { key: 'category', label: 'Category', tdClass: 'category-col' },
        { key: 'more', label: 'More', tdClass: 'more-col' }
      ]
    }
  },
  computed: {
    filteredMovies() {
      const matchesFilter = new RegExp(this.filter, 'i')
      if (this.filter) {
        return this.topmovies.filter(item => matchesFilter.test(item.title) || matchesFilter.test(item.category))
      } else {
        return this.topmovies
      }
    }
  },
  mounted() {
    axios
      .get('https://itunes.apple.com/us/rss/topmovies/limit=100/json')
      .then(
        res =>
          (this.topmovies = res.data.feed.entry.map(e => {
            return {
              title: e.title.label.split(' - ')[0],
              artist: e['im:artist'].label,
              category: e.category.attributes.label,
              released: e['im:releaseDate'].label.split('T')[0],
              id: e.id.attributes['im:id']
            }
          }))
      )
      .catch(err => console.log(err))
  }
}
</script>
