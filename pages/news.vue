<template>
  <div>
    <v-container>
      <v-flex xs12 pb-5>
        <h2>News {{ totalNews }}</h2>
      </v-flex>

      <v-flex>
        <!-- News -->
        <v-flex
          v-for="(article, index) in articlex"
          :key="index"
          sx12
          sm6
          offset-sm3
        >
          <v-card class="mx-auto mb-5">
            <v-img :src="article.urlToImage" height="200px" width="100%" />
            <v-card-title> {{ article.title }} </v-card-title>
            <v-card-subtitle> {{ article.author }} </v-card-subtitle>
            <v-card-actions>
              <v-btn color="orange lighten-2" text :href="article.url" target="_blank">
                Open
              </v-btn>
              <v-spacer></v-spacer>
              <v-btn icon @click="show = !show">
                <v-icon>{{
                  show ? 'mdi-chevron-up' : 'mdi-chevron-down'
                }}</v-icon>
              </v-btn>
            </v-card-actions>
            <v-expand-transition>
              <div v-show="show">
                <v-divider></v-divider>
                <v-card-text>
                  {{ article.description }}
                </v-card-text>
              </div>
            </v-expand-transition>
          </v-card>
        </v-flex>
      </v-flex>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'NewsPages',
  data() {
    return {
      articlex: [],
      totalNews: 0,
      show: false,
    }
  },
  mounted() {
    this.viewDataNews()
  },
  methods: {
    setNews(data) {
      this.articlex = data
    },
    viewDataNews() {
      try {
        this.$axios
          .$get(
            'v2/top-headlines?country=us&category=business&apiKey=1fddd745b31048c08d62c8e89a86eab2'
          )
          .then((response) => {
            // handle success
            console.log('Berhasil : ', response)
            this.totalNews = response.totalResults
            console.log('panjang artikel: ' + this.totalNews)
            this.setNews(response.articles)
          })
          .catch((error) => {
            // handle error
            console.log('Gagal : ', error)
          })
      } catch (error) {
        console.error('error 1 : ', error)
      }
    },
  },
}
</script>

<style>
</style>