<template>
    <div class="simon wrap">
        <div @click="submitMove('green')"
             class="green color nw click-state"
             id="green"></div>
        <div @click="submitMove('red')"
             class="red color ne click-state"
             id="red"></div>
        <div class="controls">
            <span class="control-text">
        Count: {{ numberOfMoves }}
      </span>
            <span v-if="message != ''" class="message">{{ message }}</span>
            <!-- <span class="control-text">Simon</span> -->
            <button @click="power">TURN <span v-if="powered">OFF</span><span v-else>ON</span></button>
            <button @click="startGame">Start</button>

            <button @click="toggleStrictMode">Strict<span id="strict">.</span></button>

        </div>
        <div @click="submitMove('yellow')"
             class="yellow color sw click-state"
             id="yellow"></div>
        <div @click="submitMove('blue')"
             class="blue color se click-state"
             id="blue"></div>
    </div>
</template>

<script>
export default {
  name: 'Simon',
  data () {
    return {
      powered: false,
      colors: ['green', 'red', 'yellow', 'blue'],
      numberOfMoves: 0,
      moves: [],
      strictMode: false,
      playerMoves: 0,
      gameState: '',
      playerWaitTime: 3000,
      waitTime: 750,
      sounds: [
        'https://s3.amazonaws.com/freecodecamp/simonSound1.mp3',
        'https://s3.amazonaws.com/freecodecamp/simonSound2.mp3',
        'https://s3.amazonaws.com/freecodecamp/simonSound3.mp3',
        'https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'
      ],
      message: 'Simon',
      messages: ['Wrong color!', 'Out of time!', 'You won!', 'Simon']
    }
  },
  methods: {
    power () {
      this.powered = !this.powered
      this.numberOfMoves = 0
      this.moves = []
      if (this.strictMode) {
        this.toggleStrictMode()
      }
      this.message = this.messages[3]
      this.gameState = ''
    },
    startGame () {
      if (!this.powered) {
        return
      }
      if (this.numberOfMoves > 0) {
        return
      }
      this.selectRandomColor()
    },
    toggleStrictMode () {
      this.strictMode = !this.strictMode
      let strictStatus = document.getElementById('strict')
      strictStatus.style.backgroundColor = this.strictMode ? 'red' : 'black'
    },
    gameWon () {
      this.message = this.messages[2]
      setTimeout(() => {
        this.power()
      }, 3000)
    },
    submitMove (color = '') {
      // fix to remove assignment to empty string
      if (color !== '') {
        this.lightColor(color)
      }
      if (this.moves[this.playerMoves] === color) {
        this.playerMoves++

        this.playerInput(this.waitTime)

        if (this.moves.length === this.playerMoves) {
          this.playerMoves = 0
          this.selectRandomColor()
        }
      } else {
        this.message = this.messages[0]
        if (this.strictMode) {
          setTimeout(() => {
            this.message = this.messages[3]
            this.numberOfMoves = 0
            this.moves = []
            this.selectRandomColor()
          }, 500)
          return
        }
        setTimeout(() => {
          this.message = this.messages[3]
          this.showMoves()
        }, 1000)
      }
    },
    selectRandomColor () {
      let randomColor = Math.floor(Math.random() * Math.floor(4))
      this.moves.push(this.colors[randomColor])
      this.numberOfMoves++
      setTimeout(() => {
        this.showMoves()
      }, this.waitTime)
    },
    lightColor (color) {
      let selectedColor = document.getElementById(color)
      selectedColor.classList.toggle(`light-${color}`)
      this.playSound(color)
      setTimeout(() => {
        selectedColor.classList.toggle(`light-${color}`)
      }, this.waitTime)
    },
    playSound (color) {
      new Audio(this.sounds[this.colors.indexOf(color)]).play()
    },
    showMoves () {
      this.gameState = 'playSequence'
      if (this.moves.length === 20) {
        return this.gameWon()
      }
      let move = 0
      let showSequence = setInterval(() => {
        this.lightColor(this.moves[move])
        move++
        if (move === this.moves.length) {
          clearInterval(showSequence)
        }
      }, this.waitTime)
      this.playerInput(this.waitTime)
    },
    playerInput (wait) {
      this.gameState = 'playerTurn'
      let delay = setInterval(() => {
        if (this.gameState === 'playerTurn') {
          clearInterval(delay)
          return
        }
        this.outOfTime()
        clearInterval(delay)
      }, this.playerWaitTime)
    },
    outOfTime () {
      this.message = this.messages[2]
      this.power()
    },
    toggleClickState (state) {
      for (let color of this.colors) {
        let selectedColor = document.getElementById(color)
        if (state === 'off') {
          selectedColor.classList.add('click-state')
        } else {
          selectedColor.classList.remove('click-state')
        }
      }
    }
  },
  watch: {
    gameState () {
      if (this.gameState === 'playerTurn') {
        this.toggleClickState('on')
      }
      if (this.gameState === 'playSequence' || this.gameState === '') {
        this.toggleClickState('off')
      }
    }
  }
}
</script>

<style>
.controls {
    display: flex;
    justify-content: space-around;
}
.control-text,
.message {
    font-weight: bold;
    color: white;
}
.color {
    display: inline-block;
    position: relative;
    width: 200px;
    height: 200px;
    background-color: grey;
    border: 12px solid #333;
}
/*green*/
.nw {
    border-top-left-radius: 100%;
}
/*red*/
.ne {
    border-top-right-radius: 100%;
}
/*blue*/
.se {
    border-bottom-right-radius: 100%;
}
/*yellow*/
.sw {
    border-bottom-left-radius: 100%;
}

.wrap {
    width: 472px;
    height: 472px;
    border-radius: 100%;
    position: relative;
    text-align: center;
    margin: auto;
    background-color: #333;
    top: 10vh;
    box-shadow: 0px 0px 12px #222;
}
.wrap-in {
    position: relative;
    top: 12px;
}

/*colors*/
.blue {
    background-color: blue;
}
.light-blue {
    background-color: #87cefa;
}
.yellow {
    background-color: yellow;
}
.light-yellow {
    background-color: #ffffe0;
}
.green {
    background-color: green;
}
.light-green {
    background-color: #90ee90;
}
.red {
    background-color: red;
}
.light-red {
    background-color: #ef3d47;
}

#strict {
    margin-left: 3px;
    display: inline-block;
    width: 13px;
    height: 13px;
    background: black;
    border-radius: 100%;
}
.click-state {
    pointer-events: none;
}
</style>
