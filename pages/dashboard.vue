<template lang="pug">
  v-row
    v-col(cols="3")
      v-card.tag.num.text-center
        div
          v-icon(style="font-size: 40px") mdi-thumb-up
          h3 {{dash.num_likes}}
    v-col(cols="3")
      v-card.tag.num.text-center
        v-icon.float(style="font-size: 40px") mdi-thumb-down
        h3.float {{dash.num_dislikes}}
    v-col(cols="3")
      v-card.tag.moto.text-center
        p {{dash.most_liked.name}} ({{dash.most_liked.model_year}})
        p {{dash.most_liked.likes}}
    v-col(cols="3")
      v-card.tag.moto.text-center
        p {{dash.most_disliked.name}} ({{dash.most_disliked.model_year}})
        p {{dash.most_disliked.dislikes}}
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'InspirePage',
  layout: 'private',
  middleware: ['auth'],
  data() {
    return {
      dash: []
    }
  },
  async fetch() {
    await this.$axios.$get('/variant/dash').then( (res) => {
      this.dash = res;
      console.log(res);
    });
  }
}
</script>

<style scoped>
.tag {
  height: 150px;
  margin-top: 20px;
}
.tag .img {
  height: 150px;
  width: 150px;
}
.tag > p {
  font-size: 26px;
}
.tag.num {
  font-size: 30px;
  padding-top: 30px;
}

.tag.moto {
  padding-top: 30px;
}

</style>