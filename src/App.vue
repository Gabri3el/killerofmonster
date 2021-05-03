<template>
  <div id="app">
    <div class="welcome" v-if="!start">
      <h1>Welcome to Killer of Monsters </h1>
      <p>Where the strongest survive!!!!</p>
      <button @click="startGame" class="btn newGame">Iniciar Jogo</button>
    </div>
    <div v-if="start" class="panel scores">
      <div class="score">
        <h1>Player</h1>
        <div class="lifebar">
          <div
            class="life"
            :class="{ danger: playerLife <= 20 }"
            :style="{ width: playerLife + '%' }"
          ></div>
          {{ playerLife }}%
        </div>
      </div>
      <div v-if="start" class="score">
        <h1>Monster</h1>
        <div class="lifebar">
          <div
            class="life"
            :class="{ danger: monsterLife <= 20 }"
            :style="{ width: monsterLife + '%' }"
          ></div>
        </div>
        {{ monsterLife }}%
      </div>
    </div>
  <div v-if="start && playerLife == 0 || monsterLife == 0 " class="panel result">
    <div v-if="monsterLife == 0" class="win">You Win =)</div>
    <div v-else-if="playerLife == 0" class="lose">You Loseee =(</div>
  </div>
  <div v-if="start" class="panel buttons">
    <template  v-if="monsterLife > 0 && playerLife > 0 && start">
      <button class="btn attack" @click="attack(false)">Attack</button>
      <button class="btn specialAttack" @click="attack(true)" >Special Attack</button>
      <button class="btn heal" @click="HealAndHurt">Heal</button>
    </template>
    <template v-else-if="monsterLife == 0 || playerLife == 0 && start">
      <button @click="RestartGame"  class="btn newGame">Play Again</button>
    </template>
  </div>
    
  <div v-if="start && logs.length" class="panel logs ">
    <ul>
      <li v-for="log in logs" v-bind:key="log" :class="log.cls" class="log">
        {{log.text}}
      </li>
    </ul>
  </div>
  
  
  <div v-if="start" class="start">
    <button @click="backTitleScreen" class="btn">Voltar</button>
  </div>
</div>
</template>

<script>
export default {
  name: "App",
  components: {
    
  },
  data() {
    return{
      playerLife: 100,
      monsterLife: 100,
      start: false,
      logs: []  
    }
  },
   methods: {
        startGame(){
          this.start = true;
        },
        RestartGame(){
          if(this.playerLife == 0 || this.monsterLife == 0 ){
          this.playerLife = 100
          this.monsterLife = 100
           this.logs = []
          }
        },
        backTitleScreen(){
          this.start = false;
        },
        attack(special){
          this.hurt('monsterLife',5 ,10 , special, 'Player', 'Monster', 'Player')
          if(this.monsterLife > 0){
            this.hurt('playerLife',7 ,12 , false, 'Monster', 'Player','Monster')
          }
        },
        getRandom(min,max){
          const value = Math.random() * (max - min) + min
          return Math.round(value)
        },
        hurt(prop, min, max, special, source , target, cls){
          const plus = special ? 5 : 0
          const hurt = this.getRandom(min + plus, max + plus)
          this[prop] = Math.max(this[prop] - hurt, 0)
          this.registerLogs(`${source} atingiu ${target} com ${hurt}.`,cls)
        },
        HealAndHurt(){
          this.heal(10,15)
          this.hurt('playerLife',7 ,12 , false , 'Monster', 'Player','Monster')
        },
        heal(max, min){
          const heal = this.getRandom(min,max)
          this.playerLife = Math.min(this.playerLife + heal, 100)
          this.registerLogs(`player receives ${heal} of Heal`,'Player')
        },
        registerLogs(text , cls){
          this.logs.unshift({text,cls})
          console.log(this.logs)
        }
  }, 
};
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
html {
  font-family: 'Press Start 2P', cursive;
  background-image: url('./assets/background.jpg');
  color: #fff;
}

#app {
    display: flex;
    flex-direction: column;
}
.welcome{
  margin: 100px;
  text-align: center;
}
.start{
  display: flex;
  align-items: center;
  justify-content: center;
}

.start button{
  border: 2px solid green;
  border-radius: 10px;
  background-color: green;
  color: aliceblue;
  width: 200px;
  height: 50px;
  cursor: pointer;
  font-family: 'Press Start 2P', cursive;
}

.panel {
    margin: 10px;
    padding: 20px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
    border: 2px solid white;
}


/* Scores */

.scores {
    display: flex;
}

.score {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.score h1 {
 font-size: 20px;
}

.lifebar {
    width: 70%;
    height: 30px;
    border: 2px solid #AAA;
    margin: 0;
}

.lifebar .life {
    display: flex;
    height: 100%;
    width: 100%;
    background-color: green;
}

.lifebar .life.danger {
    background-color: red;
}
/*Result */


.result{
  display: flex;
  justify-content: center;
  font-size: 1.3rem;
  font-weight: 600;
}

.result .win {
  color: green;
}

.result .lose{
  color: red;
}


/*Game Button*/
.btn{
  padding: 10px 50px;
  margin: 0 65px;
  text-transform: uppercase;
  border-radius: 5px;
  font-size: 0.8rem;
  cursor: pointer;
  font-family: 'Press Start 2P', cursive;
}
.btn button{
  display: flex;
  justify-content: center;
}

.newGame{
  background-color: #4253af;
  color:aliceblue;
  margin: 0
}

.attack{
  background-color: red;
  color: aliceblue;
}
.specialAttack{
  background-color: rgb(27, 21, 12);
  color: aliceblue;
}
.heal{
  background-color: green;
  color: aliceblue;
}


/* LOGS*/

.logs ul {
  display: flex;
  flex-direction: column;
  list-style: none;
  padding: 0;
  margin: 0 
}


.logs ul li{
  display: flex;
  justify-content: center;
  margin: 4px 0px;
  padding: 3px 0px;
  font-size: 1.1rem;
  text-transform: uppercase;
  border-radius: 3px;
}

.Player{
  background-color: #4253afaa;
  color: #fff;
}

.Monster{
  background-color: #e51c23aa;
  color: #fff
}
</style>
