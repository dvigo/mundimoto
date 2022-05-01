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
      canvas(id="likeChart", class="chart")
      canvas(id="dislikeChart", class="chart")
</template>

<script>
export default {
  name: 'InspirePage',
  layout: 'private',
  middleware: ['auth'],
   head: {
    title: 'My awesome project', // Other meta information
    script: [
      { hid: 'chartjs', src: 'https://cdn.jsdelivr.net/npm/chart.js', defer: true }
    ]
   },
  data() {
    return {
      dash: [],
      likeColumns: [],
      dislikeCol: [],
     categories: [],
    }
  },
  async fetch() {
    await this.$axios.$get('/variant/dash').then( (res) => {
      this.dash = res;
      for(let cat in this.dash.like_categories) {
        this.likeColumns.push(this.dash.like_categories[cat])
        this.categories.push(cat)
      }
      this.dislikeCol = [];
      for(let cat in this.dash.like_categories) {
        this.dislikeCol.push(this.dash.dislike_categories[cat])
      }
    });
  },
  mounted() {
    this.generateChart(this.likeColumns, 'chart')
  },
  methods: {
    generateChart(columns, id) {
        const labels = this.categories;
        const data = {
          labels: labels,
          datasets: [{
            label: 'My First Dataset',
            data: this.likeColumns,
            backgroundColor: [
              'rgba(255, 99, 132, 0.2)',
              'rgba(255, 159, 64, 0.2)',
              'rgba(255, 205, 86, 0.2)',
              'rgba(75, 192, 192, 0.2)',
              'rgba(54, 162, 235, 0.2)',
              'rgba(153, 102, 255, 0.2)',
              'rgba(201, 203, 207, 0.2)'
            ],
            borderColor: [
              'rgb(255, 99, 132)',
              'rgb(255, 159, 64)',
              'rgb(255, 205, 86)',
              'rgb(75, 192, 192)',
              'rgb(54, 162, 235)',
              'rgb(153, 102, 255)',
              'rgb(201, 203, 207)'
            ],
            borderWidth: 1
          }]
        };

        const data_dislike = {
          labels: labels,
          datasets: [{
            label: 'My First Dataset',
            data: this.dislikeCol,
            backgroundColor: [
              'rgba(255, 99, 132, 0.2)',
              'rgba(255, 159, 64, 0.2)',
              'rgba(255, 205, 86, 0.2)',
              'rgba(75, 192, 192, 0.2)',
              'rgba(54, 162, 235, 0.2)',
              'rgba(153, 102, 255, 0.2)',
              'rgba(201, 203, 207, 0.2)'
            ],
            borderColor: [
              'rgb(255, 99, 132)',
              'rgb(255, 159, 64)',
              'rgb(255, 205, 86)',
              'rgb(75, 192, 192)',
              'rgb(54, 162, 235)',
              'rgb(153, 102, 255)',
              'rgb(201, 203, 207)'
            ],
            borderWidth: 1
          }]
        };
        const config = {
            type: 'bar',
            data: data,
            options: {
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            },
        };
        const myChart = new Chart(
            document.getElementById('likeChart'),
            config
        );
        config['data'] = data_dislike
        const myChart2 = new Chart(
            document.getElementById('dislikeChart'),
            config
        );

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

canvas {
    width: 45% !important;
    height: 45% !important;
    }

</style>

