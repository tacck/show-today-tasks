<template>
  <v-container fluid>
    <v-data-iterator
      :items="lists"
      hide-default-footer
    >
      <template v-slot:header>
        <v-toolbar
          class="mb-2"
          color="indigo darken-5"
          dark
          flat
        >
          <v-toolbar-title>Today's Tasks</v-toolbar-title>
        </v-toolbar>
      </template>
      <template v-slot:default="props">
        <v-row>
          <v-col
            v-for="item in props.items"
            :key="item.name"
            cols="12"
            sm="6"
            md="4"
            lg="3"
          >
            <v-card
              :id="item.id"
              v-on:click="finish( item.id )"
            >
              <v-card-title>
                <h4>{{ item.name }}</h4>
              </v-card-title>
              <v-divider></v-divider>
              <v-list dense>
                <v-list-item>
                  <v-list-item-content>Due:</v-list-item-content>
                  <v-list-item-content class="align-end">{{ item.due }}</v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>
      </template>
    </v-data-iterator>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  props: ['listId', 'apiKey', 'apiToken'],
  data: function () {
    return {
      itemsPerPageOptions: [],
      itemsPerPage: 5,
      lists: []
    }
  },
  mounted: async function () {
    // window.interactiveCanvas.ready({
    //   onUpdate: (data) => {
    //     console.log('onUpdate: ' + data)
    //   },
    //   onTtsMark: (markName) => {
    //     console.log('onTtsMark: ' + markName)
    //   }
    // })
    window.interactiveCanvas.ready({
      onUpdate: (data) => {
        console.log('onUpdate: ' + data)
      }
    })

    const response = await axios.get(
      `https://api.trello.com/1/lists/${this.listId}/cards?fields=all&key=${this.apiKey}&token=${this.apiToken}`
    )
    console.log(response)
    this.lists = response.data
  },
  methods: {
    finish: function (id) {
      console.log(id)
      window.interactiveCanvas.sendTextQuery(`finishedTask-${id}`)
      // this.lists = this.lists.filter((list) => list.id !== id)
    }
  }
}
</script>
