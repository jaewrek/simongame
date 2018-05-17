<template>
  <div class="simon wrap">
    <div @click="submitMove('green')" class="green color nw click-state" id="green"></div>
    <div @click="submitMove('red')" class="red color ne click-state" id="red"></div>
    <div class="controls">
      <span class="control-text">
        Count: {{ numberOfMoves }}
      </span>
      <span class="control-text">Simon</span>
      <button @click="power">TURN <span v-if="powered">OFF</span><span v-else>ON</span></button>
      <button @click="startGame">Start</button>
      
        <button @click="toggleStrictMode">Strict<span id="strict">.</span></button>
        

      
      
      
    </div>
    <div @click="submitMove('yellow')" class="yellow color sw click-state" id="yellow"></div>
    <div @click="submitMove('blue')" class="blue color se click-state" id="blue"></div>
    
    
    
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
      sounds: ['https://s3.amazonaws.com/freecodecamp/simonSound1.mp3',
      'https://s3.amazonaws.com/freecodecamp/simonSound2.mp3',
       'https://s3.amazonaws.com/freecodecamp/simonSound3.mp3',
       'https://s3.amazonaws.com/freecodecamp/simonSound4.mp3']
    }
  },
  methods: {
    power() {
      this.powered = !this.powered;
      this.numberOfMoves = 0;
      this.moves = [];
      if (this.strictMode){
        this.toggleStrictMode();
      }
      this.gameState = '';
    },
    startGame() {
      if(!this.powered){ return; }
      if(this.numberOfMoves > 0){ return; }
      this.selectRandomColor();
      console.log(this.moves);
    },
    toggleStrictMode() {
      this.strictMode = !this.strictMode;
      let strictStatus = document.getElementById("strict");
      strictStatus.style.backgroundColor = this.strictMode ? 'red' : 'black'
      
    },
    submitMove(color = '') {
      // this.gameState = 'playerTurn';
      // fix to remove assignment to empty string
      if(color!=''){
        this.lightColor(color);
      } 
      console.log('submitMove', color)
      if(this.moves[this.playerMoves] == color){
        console.log('matched')
        this.playerMoves++;
        // this.gameState = 'playerTurn';
        this.playerInput(this.waitTime);

        if(this.moves.length == this.playerMoves){
          this.playerMoves = 0;
          this.selectRandomColor();
        }
        // return true;
      } else {
          if (this.strictMode){
            // show message end game
            // setTimeout(()=>{
            //   alert("Wrong move! Powering off")},1000);
            this.power();
            return;
          }
          console.log("wrong color");
          // alert("Wrong color, playing sequence");
          // // show warning
          this.showMoves();
        }
      // return false;
    },
    selectRandomColor() {
      let randomColor = Math.floor(Math.random() * Math.floor(4));
      this.moves.push(this.colors[randomColor]);
      this.numberOfMoves++;
      setTimeout( () => {
       this.showMoves();
      }, this.waitTime)
    },
    lightColor(color){
      let selectedColor = document.getElementById(color);
      selectedColor.classList.toggle(`light-${color}`);
      this.playSound(color);
      setTimeout( () => {
        console.log(selectedColor);
        selectedColor.classList.toggle(`light-${color}`);
      }, this.waitTime);
    },
    playSound(color){
      let sound = new Audio(this.sounds[this.colors.indexOf(color)]).play();
    },
    showMoves() {
      this.gameState = 'playSequence';
      console.log(this.moves);
      // this.toggleClickState();
      let move = 0;
      let showSequence = setInterval( () => {
        this.lightColor(this.moves[move]);
        move++;
        console.log(this.moves.length, 'length')
        console.log(move,'move')
        if (move === this.moves.length){
          console.log('clearsequence')
          clearInterval(showSequence);
          // this.toggleClickState();
        }
      }, this.waitTime)   
      this.playerInput(this.waitTime);
    },
    playerInput(wait) {
      this.gameState = 'playerTurn';
      //change to set interval and clear when move is submitted
      let delay = setInterval( () => {
        if (this.gameState === 'playerTurn'){
          clearInterval(delay);
          return;
        }
          this.outOfTime();
          clearInterval(interval);
      }, this.playerWaitTime);
    },
    outOfTime() {
      // show message game over
      this.power();
      console.log("Out of time")
    },
    toggleClickState(state) {
      for(let color of this.colors){
        let selectedColor = document.getElementById(color);
        if(state === 'off'){ 
          selectedColor.classList.add('click-state');
        } else {
          selectedColor.classList.remove('click-state');
        }
       
      }
    },

  },
    watch: {
    gameState() {
      if (this.gameState === 'playerTurn'){
        this.toggleClickState('on');
      }
      if (this.gameState === 'playSequence' ||
        this.gameState === ''){
        this.toggleClickState('off');
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
.control-text {
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
  border-top-left-radius : 100%;
}
/*red*/
.ne {
  border-top-right-radius : 100%;
}
/*blue*/
.se {
  border-bottom-right-radius : 100%;
}
/*yellow*/
.sw {
  border-bottom-left-radius : 100%;
}

.wrap{
  width: 472px;
  height:472px;
  border-radius:100%;
  position: relative;
  text-align: center;
  margin:auto;
  background-color:#333;
  top:10vh;
  box-shadow: 0px 0px 12px #222;
}
.wrap-in{
  position: relative;
  top:12px;
}

/*colors*/
.blue {
  background-color: blue;
}
.light-blue {
  background-color: #87CEFA;
}
.yellow {
  background-color: yellow;
}
.light-yellow {
  background-color: #FFFFE0;
}
.green {
  background-color: green;
}
.light-green {
  background-color: #90EE90;
}
.red {
  background-color: red;
}
.light-red {
  background-color: #EF3D47;
}

#strict {
margin-left: 3px;
display: inline-block;
width: 13px;
height: 13px;
background: black;
border-radius: 100%;
}
.click-state{
  pointer-events: none;
}
</style>
