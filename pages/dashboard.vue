<template lang="pug">
  v-container
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
    v-row
      div(id="chart")
</template>

<script>
export default {
  name: 'InspirePage',
  layout: 'private',
  middleware: ['auth'],
   head: {
    title: 'My awesome project', // Other meta information
    script: [
      { hid: 'c3', src: 'https://cdnjs.cloudflare.com/ajax/libs/c3/0.7.20/c3.min.js', defer: true }
    ]
   },
  data() {
    return {
      dash: [],
      likeColumns: [],
      dislikeCol: []
    }
  },
  async fetch() {
    await this.$axios.$get('/variant/dash').then( (res) => {
      this.dash = res;
      console.log(res);
      let likeColumns = [];
      for(let cat in this.dash.like_categories) {
        likeColumns.push([cat, this.dash.like_categories[cat]])
      }
      this.likeColumns = likeColumns;
      let dislikeCol = [];
      for(let cat in this.dash.like_categories) {
        dislikeCol.push([cat, this.dash.dislike_categories[cat]])
      }
      this.dislikeCol = dislikeCol;
    });
  },
  mounted() {
    this.generateChart(this.likeColumns, 'chart')
  },
  methods: {
    generateChart(columns, id) {
      c3.generate({
        bindto: '#chart',
        data: {
            columns: [
                 ['data1', 30, 200, 100, 400, 150, 250],
            ['data2', 130, 100, 140, 200, 150, 50]
            ],
            type: 'bar'
        },
        bar: {
            width: {
                ratio: 0.5 // this makes bar width 50% of length between ticks
            }
            // or
            //width: 100 // this makes bar width 100px
        }
    });
    }
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

