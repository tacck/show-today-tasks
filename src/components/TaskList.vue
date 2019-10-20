<template>
  <v-container fluid>
    <v-data-iterator :items="list" hide-default-footer>
      <template v-slot:header>
        <v-toolbar class="mb-2" color="indigo darken-5" dark flat>
          <v-toolbar-title>Today's Tasks</v-toolbar-title>
        </v-toolbar>
      </template>
      <template v-slot:default="props">
        <v-row>
          <v-col v-for="item in props.items" :key="item.name" cols="12" sm="6" md="4" lg="3">
            <v-card :id="item.id" v-on:click="finish( item.id )">
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
  props: ['listId', 'finishedListId', 'apiKey', 'apiToken'],
  data: function () {
    return {
      itemsPerPageOptions: [],
      itemsPerPage: 5,
      list: []
    }
  },
  mounted: async function () {
    // Callback で カード移動の処理を実装
    window.interactiveCanvas.ready({
      onUpdate: async data => {
        let cardId = ''
        let cardNumber = -1

        if ('cardId' in data) {
          cardId = data.cardId
          cardNumber = this.list.findIndex(element => {
            return element.id === cardId
          })
        } else if ('cardNumber' in data) {
          cardNumber = data.cardNumber - 1
          const card = this.list[cardNumber]
          cardId = card.id
        } else {
          // 更新不要なので終了
          return
        }

        await axios.put(
          `https://api.trello.com/1/cards/${cardId}?idList=${
            this.finishedListId
          }&key=${this.apiKey}&token=${this.apiToken}&pos=bottom`
        )
        this.list.splice(cardNumber, 1)
      },
      onTtsMark: markName => {}
    })

    const response = await axios.get(
      `https://api.trello.com/1/lists/${this.listId}/cards?fields=all&key=${
        this.apiKey
      }&token=${this.apiToken}`
    )
    this.list = response.data
  },
  methods: {
    finish: function (id) {
      window.interactiveCanvas.sendTextQuery(`finishedTask-${id}`)
    }
  }
}
</script>
