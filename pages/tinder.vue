 <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
<template>
  <section class="container">
    <div class="fixed header">
      <span></span>
      <span>Motonder</span>
      <span></span>
    </div>
    <div id="fun"></div>
    <div
      v-if="current"
      class="fixed fixed--center"
      style="z-index: 3"
      :class="{ 'transition': isVisible }">
      <Vue2InteractDraggable
        v-if="isVisible"
        :interact-out-of-sight-x-coordinate="500"
        :interact-max-rotation="15"
        :interact-x-threshold="200"
        :interact-y-threshold="200"
        :interact-event-bus-events="interactEventBus"
        interact-block-drag-down
        interact-block-drag-up
        @draggedRight="emitAndNext('match', next.id)"
        @draggedLeft="emitAndNext('reject', next.id)"
        class="rounded-borders card card--one">
        <div style="height: 100%">
          <img
            :src="current.image"
            class="rounded-borders"/>
        <div class="upper-text">
            <h2>{{next.extra_data['category']}}</span></h2>
          </div>
          <div class="text">
            <h2>{{current.name}}, <span>{{current.brand}}</span></h2>
          </div>
        </div>
      </Vue2InteractDraggable>
    </div>
    <div
      v-if="next"
      class="rounded-borders card card--two fixed fixed--center"
      style="z-index: 2">
      <div style="height: 100%">
        <img
          :src="next.image"
          class="rounded-borders"/>
        <div class="upper-text">
            <h2>{{next.extra_data['category']}}</span></h2>
          </div>
        <div class="text">
            <h2>{{next.name}}, <span>{{next.brand}}</span></h2>
          </div>
      </div>
    </div>
    <div
      v-if="index + 2 < cards.length"
      class="rounded-borders card card--three fixed fixed--center"
      style="z-index: 1">
      <div style="height: 100%">
      </div>
    </div>
    <div class="footer fixed">
      <div class="btn btn--decline" @click="reject">
          <i class="material-icons">close</i>
      </div>
      <div class="btn btn--like" @click="match">
      <i class="fas fa-file"></i>
          <i class="material-icons">favorite</i>
      </div>
    </div>
  </section>
</template>

<script>
import { Vue2InteractDraggable, InteractEventBus } from 'vue2-interact'
import {  emojisplosions } from "emojisplosion";
const EVENTS = {
  MATCH: 'match',
  REJECT: 'reject'
}
export default {
  name: 'SwipeableCards',
  middleware: ['auth'],
  components: { Vue2InteractDraggable },
  data() {
    return {
      isVisible: true,
      index: 0,
      interactEventBus: {
        draggedRight: EVENTS.MATCH,
        draggedLeft: EVENTS.REJECT,
      },
      cards: []
    }
  },
  async created() {
    await this.$axios.$get('/variant?page=0&page_size=27').then( (res) => this.cards = res);
    document.onkeydown = function (event) {
      switch (event.keyCode) {
         case 37:
            document.querySelector(".btn--decline").click();
            break;
         case 39:
            document.querySelector(".btn--like").click();
            break;
           break;
      }
   };
  },
  computed: {
    current() {
      return this.cards[this.index]
    },
    next() {
      return this.cards[this.index + 1]
    }
  },
  methods: {
   match() {
      InteractEventBus.$emit(EVENTS.MATCH);
    },
    reject() {
      InteractEventBus.$emit(EVENTS.REJECT)
    },
    emitAndNext(event, variant_id) {
      if (event == 'match') {
          var self = this;
          this.$axios.$post('/variant/swinger', {liked: true, variant: variant_id}).then(function(data) {
              self.cards.forEach(function(card) {
                  self.cards.push(card);
              });
              self.cards = self.cards.filter((value, index, self) =>
                index === self.findIndex((t) => (
                t.id === value.id
                ))
              )
          });
          const { cancel } = emojisplosions({
            position: {x: window.innerWidth / 2, y:window.innerHeight / 2},
            emojiCount: 20,
            emojis: ["🔥", "🏍", "🛵"]
          });
          setTimeout(cancel, 100);
      } else if (event == 'reject') {
          var self = this;
          this.$axios.$post('/variant/swinger', {disliked: true, variant: variant_id}).then(function(data) {
              cards.forEach(function(card) {
                  self.cards.push(card);
              });
          });
      }
      this.$emit(event, this.index)
      setTimeout(() => this.isVisible = false, 200)
      setTimeout(() => {
        this.index++
        this.isVisible = true
      }, 200)
    }
  }
}
</script>


<style lang="scss" scoped>
.header {
  width: 100%;
  height: 100%;
  z-index: 0;
  top: 50px;
  left: 0;
  color: white;
  text-align: center;
  font-family: 'Engagement';
  background: #C1281E;
    background: -webkit-linear-gradient(to top, #1e1e1d, 60%, #C1281E);
  background: linear-gradient(to top, #1e1e1d, 60%, #C1281E);
  display: flex;
  justify-content: space-between;
  span {
    display: block;
    font-size: 4rem;
    padding-top: 2rem;
    text-shadow: 1px 1px 1px red;
  }
  i {
    padding: 24px;
  }
}
.footer {

  width: 77vw;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  padding-bottom: 30px;
  justify-content: space-around;
  margin-top: 100px;
  align-items: center;
}
.btn {
  position: relative;
  width: 70px;
  height: 70px;
  padding: .2rem;
  border-radius: 50%;
  background-color: white;
  box-shadow: 0 6px 6px -3px rgba(0,0,0,0.02), 0 10px 14px 1px rgba(0,0,0,0.02), 0 4px 18px 3px rgba(0,0,0,0.02);
  cursor: pointer;
  transition: all .3s ease;
  user-select: none;
  -webkit-tap-highlight-color:transparent;
  &:active {
    transform: translateY(4px);
  }
  i {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    &::before {
      content: '';
    }
  }
  &--like {
    background-color: red;
    padding: .5rem;
    color: white;
    box-shadow: 0 10px 13px -6px rgba(0,0,0,.2), 0 20px 31px 3px rgba(0,0,0,.14), 0 8px 38px 7px rgba(0,0,0,.12);
    i {
      font-size: 32px;
    }
  }
  &--decline {
    color: red;
  }
  &--skip {
    color: green;
  }
}
.flex {
  display: flex;
  &--center {
    align-items: center;
    justify-content: center;
  }
}
.fixed {
  position: fixed;
  &--center {
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
.rounded-borders {
  border-radius: 12px;
}
.card {
  width: 40vw;
  height: 60vh;
  color: white;
  img {
    object-fit: cover;
    display: block;
    width: 100%;
    height: 100%;
  }
  &--one {
    box-shadow: 0 1px 3px rgba(0,0,0,.2), 0 1px 1px rgba(0,0,0,.14), 0 2px 1px -1px rgba(0,0,0,.12);
  }
  &--two {
    transform: translate(-48%, -48%);
    box-shadow: 0 6px 6px -3px rgba(0,0,0,.2), 0 10px 14px 1px rgba(0,0,0,.14), 0 4px 18px 3px rgba(0,0,0,.12);
  }
  &--three {
    background: rgba(black, .8);
    transform: translate(-46%, -46%);
    box-shadow: 0 10px 13px -6px rgba(0,0,0,.2), 0 20px 31px 3px rgba(0,0,0,.14), 0 8px 38px 7px rgba(0,0,0,.12);
  }
  .upper-text {
    position: absolute;
    top: 0;
    width: 100%;
    text-align: right;
    background: #C1281E;
    background:rgba(0,0,0,0.5);
    text-indent: 20px;
        padding-right:20px;
    span {
      font-weight: normal;
    }
  }
  .text {
    position: absolute;
    bottom: 0;
    width: 100%;
    background:black;
    background:rgba(0,0,0,0.5);
    border-bottom-right-radius: 12px;
    border-bottom-left-radius: 12px;
    text-indent: 20px;
    span {
      font-weight: normal;
    }
  }
}
.transition {
  animation: appear 200ms ease-in;
}
@keyframes appear {
  from {
    transform: translate(-48%, -48%);
  }
  to {
    transform: translate(-50%, -50%);
  }
}
@import url('https://fonts.googleapis.com/css?family=Engagement');
@import url('https://fonts.googleapis.com/css?family=Material+Icons');

.material-icons {
  font-family: 'Material Icons';
  font-weight: normal;
  font-style: normal;
  font-size: 30px;  /* Preferred icon size */
  display: inline-block;
  line-height: 1;
  text-transform: none;
  letter-spacing: normal;
  word-wrap: normal;
  white-space: nowrap;
  direction: ltr;

  /* Support for all WebKit browsers. */
  -webkit-font-smoothing: antialiased;
  /* Support for Safari and Chrome. */
  text-rendering: optimizeLegibility;

  /* Support for Firefox. */
  -moz-osx-font-smoothing: grayscale;

  /* Support for IE. */
  font-feature-settings: 'liga';
}
</style>
