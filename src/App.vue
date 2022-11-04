<template>
  <div class="app">
  <h1>Ну что же, приступим?</h1>
  <div class="circle-block">
    <transition-group name="animation-element">
      <div class="circle-element"
        v-for="block in circleBlock"
        :key="block.id"
        @click="[swapColor(block), subAnimation(block), pickUp(block)]"
        :class="[{ darkslateblue: block.notActive, rotateUp: block.rotate, nonTarget: block.notTarget }, block.name ]" 
        >
      </div>
    </transition-group>
  </div>
  <div class="info"
  v-if="sortingCycle">Кручу-верчу, обмануть хочу!</div>
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
  v-show="inGame === false"
  @click="startGame"
  >Start</button>
  </div>

  </template>
  
  <script>

    export default {
      data() {
        return {
          circleBlock: [
            {id: Math.ceil(Math.random() * 1000), name: 'red', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'yellow', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'green', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'pink', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'blue', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'white', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'red', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'yellow', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'green', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'pink', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'blue', notActive: true, rotate: false, notTarget: true },
            {id: Math.ceil(Math.random() * 1000), name: 'white', notActive: true, rotate: false, notTarget: true }
            
          ],
          limitCounter: 10,
          cacheCombo: [],
          finishLine: [],
          inGame: false,
          sortingCycle: false,
          gameLose: false,
          gameWin: false
        }
      },
      methods: {
        
        swapColor(block) {
          block.notActive === false ? block.notActive = true : block.notActive = false
        },

        pickUp(block) {
          block.notTarget = true
          this.cacheCombo.push(block)
          if (this.cacheCombo.length == 2) {
            if(this.cacheCombo[0].name === this.cacheCombo[1].name) {
              this.finishLine.push(this.cacheCombo)
                if (this.finishLine.length === 6) {
                  this.gameWin = true
              } 
              this.cacheCombo = []
            } else {
              for (let i = 0; i < 2; ++i) {
                let result = this.circleBlock[this.circleBlock.findIndex(x =>  x.id === this.cacheCombo[i].id )]
                setTimeout(() => {
                  result.rotate = true
                  result.notActive = true
                  result.notTarget = false
                }, 1000); 
              } 
              this.cacheCombo = []
              this.limitCounter--
              if (this.limitCounter === 0) {
                this.gameLose = true
                this.circleBlock.map(x => x.notTarget = true)
              }
            } 
          }

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
  
        subAnimation(block) {
          block.rotate = true
          setTimeout(() => {
            block.rotate = false
          }, 1000);
        },
  
        cycleAnimation(){
          this.circleBlock.map(a => a.notActive = false)
          this.circleBlock.map(a => a.rotate = true)
          setTimeout(() => {
            this.circleBlock.map(a => a.rotate = false)
          }, 1000);
          
          setTimeout(() => {
            this.circleBlock.map(a => a.notActive = true)
            this.circleBlock.map(a => a.rotate = true)
            setTimeout(() => {
              this.circleBlock.map(a => a.rotate = false)
            }, 1000);
          }, 1000);
        },
  
        startGame() {
          this.sortingCycle = true
          this.cycleAnimation()
          this.inGame = true
  
          setTimeout(() => {
            this.sortNameUp()
            setTimeout(() => {
              this.cycleAnimation()
              setTimeout(() => {
                this.sortIdUp()
                setTimeout(() => {
                  this.cycleAnimation()
                  setTimeout(() => {
                    this.sortNameDown()
                    setTimeout(() => {
                      this.cycleAnimation()
                      setTimeout(() => {
                        this.sortIdDown()
                        setTimeout(() => {
                          this.sortingCycle = false
                          this.circleBlock.map(x => x.notTarget = false)
                        }, 1500);
                      }, 2000);
                    }, 1500);
                  }, 2000);
                }, 1500);
              }, 2000);
            }, 1500);
          }, 2000);
        },
        
        nextTry() {
          this.gameLose = false
          this.gameWin = false
          this.finishLine = []
          this.limitCounter = 10
          this.circleBlock.map(x => x.id = Math.ceil(Math.random() * 1000))
          this.circleBlock.map(x => x.notTarget = false)
          this.startGame()
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
  }
  
  .yellow {
    background-color: yellow;
  }
  
  .blue {
    background-color: blue;
  }
  
  .white {
    background-color: white;
  }
  
  .green {
    background-color: green;
  }
  
  .pink {
    background-color: pink;
  }
  
  .darkslateblue {
    background-color: darkslateblue;
  }
  
  .animation-element-move {
    transition: transform 1s ease-out;
  }
  
  .rotateUp {
    transform: rotateY(180deg);
    transition-duration: 1s;
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