<template>
  <div id="main" v-bind:style="{ marginTop: marginTop + 'px' }">
    <TaskList :list-id="listId" :api-key="apiKey" :api-token="apiToken" />
  </div>
</template>

<script>
import TaskList from '../components/TaskList'

export default {
  mounted: async function () {
    this.marginTop = await window.interactiveCanvas.getHeaderHeightPx()
  },
  data: function () {
    return {
      listId: process.env.VUE_APP_TRELLO_LIST_ID,
      apiKey: process.env.VUE_APP_TRELLO_API_KEY,
      apiToken: process.env.VUE_APP_TRELLO_API_TOKEN,
      marginTop: 0
    }
  },
  components: {
    TaskList
  }
}
</script>
