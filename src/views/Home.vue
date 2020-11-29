<template>
  <div class="home">
      <div class="board">
          <span class="steps">Steps : {{ steps }}</span>
          <span class="steps">Time : {{ time }} s</span>
      </div>
      <div v-if="finish" class="finish">
          <div class="head">
              <span @click="closeFinish()" class="close">X</span>
          </div>
          <div class="body">
              <span>Steps : {{ steps }}</span>
              <span>Time : {{ time }}</span>
          </div>

      </div>
      <div class="main">
          <div v-if="!start" class="start">
              <span @click="toggleTimer()">Start</span>
          </div>
          <div v-for="(card, id) in cards" class="card">
              <img v-if="card.show" class="image" :src="'images/' + card.name + '.png'" alt="">
              <img v-else :src="'images/back.svg'" alt="" class="image" @click="showCard(card, id)">
          </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data(){
      return{
          test: 'test',
          cards: [{name: '1', show: false}, {name: '2', show: false}, {name: '3', show: false}, {name: '4', show: false}, {name: '5', show: false}, {name: '6', show: false}, {name: '7', show: false}, {name: '8', show: false}, {name: '1', show: false}, {name: '2', show: false}, {name: '3', show: false}, {name: '4', show: false}, {name: '5', show: false}, {name: '6', show: false}, {name: '7', show: false}, {name: '8', show: false}],
          temp_card: {name: null, id: null},
          show: true,
          start: false,
          finish: false,
          steps: 0,
          time: 0,
          isRunning: false,
          interval: null
      }
  },
  methods:{
      showCard(card, id){
          if(this.show === true){
              this.cards[id].show = true;
              if(this.temp_card.id === null){
                  this.temp_card.name = card.name;
                  this.temp_card.id = id;
              }else{
                  this.show = false;
                  if(this.temp_card.name === card.name){
                      this.temp_card = {name: null, id: null};
                      this.show = true;
                      this.steps++;
                      if(this.cards.findIndex(i => i.show === false) === -1){
                          this.finish = true;
                          this.toggleTimer();
                      }
                  }else{
                      setTimeout(() => {
                          this.cards[id].show = false;
                          this.cards[this.temp_card.id].show = false;
                          this.temp_card = {name: null, id: null};
                          this.show = true;
                          this.steps++;
                          // console.log(this.cards.findIndex(i => i.show === false));

                      }, 2000);
                  }
              }
          }

      },
      toggleTimer() {
          if (this.isRunning) {
              this.finish = true;
              clearInterval(this.interval);
          } else {
              this.start = true;
              this.interval = setInterval(this.incrementTime, 1000);
          }
          this.isRunning = !this.isRunning
      },
      incrementTime() {
          this.time = parseInt(this.time) + 1;
      },
      closeFinish()
      {
          this.finish = false;
          this.time = 0;
          this.steps = 0;
          this.start = false;
          this.cards = [{name: '1', show: false}, {name: '2', show: false}, {name: '3', show: false}, {name: '4', show: false}, {name: '5', show: false}, {name: '6', show: false}, {name: '7', show: false}, {name: '8', show: false}, {name: '1', show: false}, {name: '2', show: false}, {name: '3', show: false}, {name: '4', show: false}, {name: '5', show: false}, {name: '6', show: false}, {name: '7', show: false}, {name: '8', show: false}];
      }
  },
  mounted() {

      this.cards.sort(() => Math.random() - 0.5);

  }
}
</script>

<style lang="scss">
    .home
    {
        display: flex;
        justify-content: center;
        position: relative;
        .finish
        {
            position: absolute;
            width: 400px;
            height: 400px;
            background: gray;
            z-index: 10;
            flex-direction: column;
            .head
            {
                height: 50px;
                font-size: 30px;
                padding: 10px 20px;
                font-family: sans-serif;
                display: flex;
                justify-content: flex-end;
                align-items: center;

                .close
                {
                    cursor: pointer;
                }
            }
            .body
            {
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                padding-top: 100px;
            }
            span
            {
                font-size: 30px;
                font-family: sans-serif;
            }
        }
        .board
        {
            display: flex;
            flex-direction: column;
            .steps
            {
                font-size: 30px;
                font-family: sans-serif;
            }

        }

        .main
        {
            position: relative;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            width: 50%;
            height: auto;
            .card
            {
                display: inline-block;
                box-sizing: border-box;
                width: 25%;
                padding: 10px;

                .image
                {
                    width: 100%;
                }
            }
            .start
            {
                display: flex;
                justify-content: center;
                align-items: center;
                font-size: 100px;
                font-family: sans-serif;
                font-weight: bold;
                color: blue;
                position: absolute;
                width: 100%;
                height: 100%;

                span
                {
                    cursor: pointer;
                }
            }
        }
    }


</style>