<template>
  <div class="simon wrap">
    <div class="green color"></div>
    <div class="red color"></div>
    <div class="controls">
      <h3 class="control-text">Simon</h3>
      <span class="control-text">Count</span>
      <button>Start</button>
      <button>Strict</button>
      <!-- <label class="switch">
        <input type="checkbox">
          <span class="slider"></span>
      </label> -->
    </div>
    <div class="yellow color"></div>
    <div class="blue color"></div>
    
    
    
  </div>
</template>

<script>
export default {
  name: 'Simon',
  data () {
    return {
      
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
  background-color: lightblue;
}
.yellow {
  background-color: yellow;
}
.light-yellow {
  background-color: lightyellow;
}
.green {
  background-color: green;
}
.light-green {
  background-color: lightgreen;
}
.red {
  background-color: red;
}
.light-red {
  background-color: pink;
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
