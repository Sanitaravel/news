<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-view-dashboard</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-settings</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Settings</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>Новости</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-list-item-content>

      </v-list-item-content>
      <v-container
        class="fill-height"
        fluid
      >
      <v-radio-group v-model="selected_language" @change="update()" row>
          <v-radio label="English" value="us"></v-radio>
          <v-radio label="Русский" value="ru"></v-radio>
          <v-radio label="Français" value="fr"></v-radio>
          <v-radio label="Italiano" value="it"></v-radio>
          <v-radio label="Deutsch" value="de"></v-radio>
        </v-radio-group>
        <v-row
          align="center"
          justify="center"
        >

        <Card v-for="i in text" :key="i.url" :author="i.author" :title="i.title" :descr="i.description" :url_kek="i.urlToImage" :url="i.url">
        </Card>
        
        </v-row>
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
import Card from './components/Card.vue'
import axios from 'axios'

  export default {
    props: {
      source: String,
    },
    components:{
      Card
    },
    data: () => ({
      drawer: null,
      text: [],
      selected_language: 'ru'
    }),
    methods:{
      getInfo(){
        axios.get(`https://newsapi.org/v2/top-headlines?country=${this.selected_language}&apiKey=d7f41a32c26b4bbfb596d58b1a54c766`).then((response) => {
          this.text = response.data.articles;
          console.log('responce', response)
        })
      },
      update(){
        console.log("kekeke")
        this.getInfo()
      }
    },
    created () {
      this.$vuetify.theme.dark = true
    },
    mounted(){
      this.getInfo()
    }
  }
</script>