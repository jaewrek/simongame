<template>
  <div class="simon wrap">
    <div @click="submitMove('green')" class="green color" id="green"></div>
    <div @click="submitMove('red')" class="red color" id="red"></div>
    <div class="controls">
      <h3 class="control-text">Simon</h3>
      <span class="control-text">
        Count: {{ numberOfMoves }}
      </span>
      <button @click="startGame">Start</button>
      <button @click="toggleStrictMode">Strict</button>
      <span v-if="strictMode">ON</span>
      <!-- <label class="switch">
        <input type="checkbox">
          <span class="slider"></span>
      </label> -->
    </div>
    <div @click="submitMove('yellow')" class="yellow color" id="yellow"></div>
    <div @click="submitMove('blue')" class="blue color" id="blue"></div>
    
    
    
  </div>
</template>

<script>
export default {
  name: 'Simon',
  data () {
    return {
      colors: ['green', 'red', 'yellow', 'blue'],
      numberOfMoves: 0,
      moves: [],
      strictMode: false,
      playerMoves: 0,
      waitTime: 3000

    }
  },
  methods: {
    startGame() {
      this.selectRandomColor();
      console.log(this.moves);
    },
    toggleStrictMode() {
      this.strictMode = !this.strictMode;
    },
    submitMove(color = '') {
      if(color!=''){
        this.lightColor(color);
      }
      
      console.log('submitMove', color)
      if(this.moves[this.playerMoves] == color){
        console.log('matched')
        this.playerMoves++;
        this.playerInput(this.waitTime);
        if(this.moves.length == this.playerMoves){
          this.playerMoves = 0;
          this.selectRandomColor();
        }
        return true;
      }
      return false;
    },
    selectRandomColor() {
      let randomColor = Math.floor(Math.random() * Math.floor(4));
      this.moves.push(this.colors[randomColor]);
      this.numberOfMoves++;

      setTimeout( () => {
       this.showMoves();
      }, 1000)
    },
    lightColor(color){
      let selectedColor = document.getElementById(color);
      // selectedColor.removeClass(green);
      selectedColor.classList.toggle(`light-${color}`);
      setTimeout( () => {
        console.log(selectedColor);
        selectedColor.classList.toggle(`light-${color}`);
      }, 500);
    },
    showMoves() {
      console.log(this.moves);
      for(let move of this.moves){
        setTimeout( () => {
          this.lightColor(move);
        }, 1500); 
      }
      this.playerInput(this.waitTime);
    },
    playerInput(wait) {
      setTimeout( () => {
        if (this.submitMove()){
          return;
        }
          this.outOfTime();
      }, wait);
    },
    outOfTime() {

    }
  }
}
</script>


<style>
.control-text {
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
.t-l {
  border-top-left-radius : 100%;
}
/*red*/
.t-r {
  border-top-right-radius : 100%;
}
/*blue*/
.b-r {
  border-bottom-right-radius : 100%;
}
/*yellow*/
.b-l {
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
/* Hide default HTML checkbox */
/*.switch input {
  display:none;
}
*/
/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
</style>
