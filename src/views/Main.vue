<template>
  <div id="main" v-bind:style="{ marginTop: marginTop + 'px' }">
    <TaskList :list-id="listId" :finished-list-id="finishedListId" :api-key="apiKey" :api-token="apiToken" />
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
      listId: process.env.VUE_APP_TRELLO_LIST_ID || this.$route.query.listId,
      finishedListId: process.env.VUE_APP_TRELLO_FINISHED_LIST_ID || this.$route.query.finishedListId,
      apiKey: process.env.VUE_APP_TRELLO_API_KEY || this.$route.query.apiKey,
      apiToken: process.env.VUE_APP_TRELLO_API_TOKEN || this.$route.query.apiToken,
      marginTop: 0
    }
  },
  components: {
    TaskList
  }
}
</script>
