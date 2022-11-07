<template>
  <div class="app">
  <h1>Ну что же, приступим?</h1>
  <div class="circle-block">
    <transition-group name="animation-element">
      <div class="circle-element"
        v-for="block in circleBlock"
        :key="block.id"
        @click="picked(block)"
        :class="[block.notActive ? 'darkslateblue' : block.name, { nonTarget: sortingCycle || gameLose || gameWin }]" 
        >
      </div>
    </transition-group>
  </div>
  <div class="info"
  v-if="!start && sortingCycle">Кручу-верчу, обмануть хочу!</div>
  <div class="info"
  v-else-if="gameWin">Поздравляю, Вы победили!</div>
  <div class="info"
  v-else-if="gameLose">Ничего страшного, попробуйте еще раз!</div>
  <div class="info"
  v-else>Жмите!</div>
  
  <button class="btn start"
  v-show="gameLose === true || gameWin === true"
  @click="nextTry"
  >Next try</button>

  <button class="btn start"
  v-show="start"
  @click="startGame"
  >Start</button>
  </div>

  </template>
  
  <script>

    export default {
      data() {
        return {
          circleBlock: [
            {id: Math.ceil(Math.random() * 10000), name: 'red', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'yellow', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'green', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'pink', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'blue', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'white', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'red', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'yellow', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'green', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'pink', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'blue', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 10000), name: 'white', notActive: true, rotate: false, notTarget: true }
            
          ],
          limitCounter: 10,
          cacheCombo: [],
          winCombo: [],
          start: true,
          inGame: false,
          sortingCycle: true,
          gameLose: false,
          gameWin: false
        }
      },
      methods: {
        
        picked(block) {
          block.notActive === false ? block.notActive = true : block.notActive = false
          this.cacheCombo.push(block)
          if (this.cacheCombo.length === 2) {
            if (this.cacheCombo[0].name === this.cacheCombo[1].name) {
              this.winCombo.push(this.cacheCombo)
              this.cacheCombo = []
              if (this.winCombo.length === 6) {
                this.gameWin = true
              }
            } else {
              for (let i = 0; i < 2; i++) {
              let result = this.circleBlock[this.circleBlock.findIndex(x => x.id === this.cacheCombo[i].id)]
              setTimeout(() => {
                result.notActive = true
              }, 1500);
            }
                this.limitCounter--
              this.cacheCombo = []

              if (this.limitCounter === 0) {
                this.gameLose = true
              }
            }
          }
        },

        cycleAnimation(callback) {
          this.circleBlock.map(x => x.notActive = false)
          setTimeout(() => {
            this.circleBlock.map(x => x.notActive = true)
            setTimeout(() => {
              callback()
            }, 1000);
          }, 1500);
        },

        startGame() {
          this.inGame = true
          this.start = false
          this.cycleAnimation(this.sortNameUp)
          setTimeout(() => {
            this.cycleAnimation(this.sortIdUp)
            setTimeout(() => {
              this.sortingCycle = false
            }, 4000);
          }, 4000);
          
        },

        nextTry() {
          this.gameLose = false
          this.gameWin = false
          this.finishLine = []
          this.limitCounter = 10
          this.circleBlock.map(x => x.id = Math.ceil(Math.random() * 1000))
          setTimeout(() => {
            this.startGame()
          }, 1000);
        },

         sortIdUp() {
          this.circleBlock.sort((a,b) => a.id - b.id)
        },
  
        sortIdDown() {
          this.circleBlock.sort((a,b) => b.id - a.id)
        },
  
        sortNameUp() {
          this.circleBlock.sort((a,b) => a.name > b.name ? 1 : a.name < b.name ? -1 : 0)
        },
  
        sortNameDown() {
          this.circleBlock.sort((a,b) => a.name > b.name ? -1 : a.name < b.name ? 1 : 0)
        },
  
      },
  
      computed: {
  
      },
  
      watch: {
      
      },
    }
  </script>
  
  <style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
  
  .app {
    display: flex;
    flex-direction: column;
    background-color: teal;
    height: 100vh;
    align-items: center;
  }
  
  h1 {
    font-size: 50px;
    padding: 30px;
  }
  
  .circle-block {
    width: 900px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    justify-items: center;
  }
  
  .circle-element {
    width: 100px;
    height: 100px;
    border-radius: 50px;
    margin: 20px;
  }
  
  .btn {
    height: 80px;
    width: 200px;
    border: 1px solid rgb(45, 189, 45);
    border-radius: 15px;
    margin: 50px;
    background: rgb(45, 189, 45);
    font-size: 40px;
    font-weight: 900;
    font-style: italic;
    font-family: Georgia, 'Times New Roman', Times, serif;
    box-shadow: 8px 8px;
  }
  
  .btn:hover {
    transform: scale(1.1);
    transition: all 0.3s ease;
    box-shadow: 10px;
  }
  
  .btn:active {
    transform: scale(1);
    transition: all 0.2s ease;
  }
  
 
  
  .red {
    background-color: red;
    animation: circle_rotate 1s;
    pointer-events: none;
  }
  
  .yellow {
    background-color: yellow;
    animation: circle_rotate 1s;
    pointer-events: none;
  }
  
  .blue {
    background-color: blue;
    animation: circle_rotate 1s;
    pointer-events: none;
  }
  
  .white {
    background-color: white;
    animation: circle_rotate 1s;
    pointer-events: none;
  }
  
  .green {
    background-color: green;
    animation: circle_rotate 1s;
    pointer-events: none;
  }
  
  .pink {
    background-color: pink;
    animation: circle_rotate 1s;
    pointer-events: none;
  }
  
  .darkslateblue {
    background-color: darkslateblue;
    transform: rotateY(-180deg);
    transition-duration: 1s;
  }
  
  .animation-element-move {
    transition: transform 1s ease-out;
  }
  
  @keyframes circle_rotate {
  0% {
    background: darkslateblue;
  }
  25% {
    background: darkslateblue;
  }
  100% {
    transform: rotateY(180deg);
  }
  
}

  .info {
    font-size: 30px;
    font-weight: 900;
    font-style: italic;
    font-family: Georgia, 'Times New Roman', Times, serif;
    margin-top: 40px;
  }

  .nonTarget {
    pointer-events: none;
  }
  
  </style>