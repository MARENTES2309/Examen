<template>
    <div class="container">
      <div class="linea1">
        <button type="button" class="b1" ref="green" @click="colorClick('green')"></button>
        <div id="score-grid">
          <div class="js-space-switch" id="score-title">
            <p>SCORE</p>
          </div>
          <div class="js-space-switch" id="score-counter">
            <p id="score-number">{{ parseFloat(score).toFixed(2) }}</p>
          </div>
        </div>
        <button type="button" class="b2" ref="blue" @click="colorClick('blue')"></button>
      </div>  
      <div class="linea2">
        <button type="button" class="b3" ref="pink" @click="colorClick('pink')"></button>
      </div>  
      <div class="linea3">
        <button type="button" class="b4" ref="purple" @click="colorClick('purple')"></button>
        <button type="button" class="inicio" @click="startGame()">START</button>
        <button type="button" class="b5" ref="black" @click="colorClick('black')"></button>
      </div> 
    </div>
</template>

<script>
export default {
  data() {
    return {
      sequence: [],
      playerSequence: [],
      timeInterval: 1000,
      score: 0,
      currentActive: 0,
      started: false,
    };
  },
  methods: {
    startGame(){
      this.sequence = []
      this.playerSequence = []
      this.score = 0
      this.start()
    },
    async start() {
      this.started = true;
      this.createSequence();
      this.flash();
    },
    createSequence() {
      let colors = ["green", "blue", "black", "purple","pink"];
      this.sequence.push(colors[Math.floor(Math.random() * colors.length)]);
    },
    blink(event, color) {
      event.style.background = color;
      setTimeout(() => {
        event.style.background = "#FFF";
      }, 500);
    },
    flash() {
      this.currentActive = 0;
      let ctx = this;
      let interval = setInterval(function() {
        let color = ctx.sequence[ctx.currentActive];
        ctx.blink(ctx.$refs[ctx.sequence[ctx.currentActive]], color);
        ctx.currentActive++;
        if (ctx.currentActive >= ctx.sequence.length) {
          clearInterval(interval);
        }
      }, this.timeInterval);
    },
    colorClick(event) {
      this.blink(this.$refs[event], event);
      if (this.started) {
        this.playerSequence.push(event);
        this.score += this.sequence.length * 0.20;
        for (let i = 0; i < this.playerSequence.length; i++) {
          if (
            JSON.stringify(this.sequence) == JSON.stringify(this.playerSequence)
          ) {
            this.playerSequence = [];
            if (this.score >= 10) {
              alert(
                `Felicidades Ganaste "START" para inciar de nuevo!`
              );
              this.started = false
            }
            this.start();
            break;
          } else if (this.playerSequence[i] !== this.sequence[i]) {
            alert(
              `Perdiste! "START" para inciar de nuevo!`
            );
            this.started = false
            break;
          }
        }
      }
    },
  },
};

  
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.container{
  height: 100vh;
  position: relative;
}
.linea1{
  display: flex;
  justify-content: space-between;
}
.linea2{
  display: flex;
  justify-content: space-around;
}
.linea3{
  display: flex;
  justify-content: space-between;
}
.b1 {
  height: 120px;
  width: 120px;
  background-color: #FFF;
}
.b2 {
  height: 120px;
  width: 120px;
  background-color: #FFF;
}
.b3 {
  height: 120px;
  width: 120px;
  background-color: #FFF;
}
.b4 {
  height: 120px;
  width: 120px;
  background-color: #FFF;
}
.b5 {
  height: 120px;
  width: 120px;
  background-color: #FFF;
}
.inicio{
  margin-top: 20px;
  width: 100px;
  height: 50px;
}
.score-title {
  border: 2px solid black;
  background: rgb(105, 215, 250);
  grid-row: 1 / span 1;
  grid-column: 1 / span 1;
}
.score-counter {
  border: 2px solid black;
  background: white;
  border-top: 0px;
  color: rgb(221, 75, 62);
  grid-row: 2 / span 1;
  grid-column: 1 / span 1;
}
</style>
