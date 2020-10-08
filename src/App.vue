<template>
  <div id="app">
    <div id="gameScreen" v-bind:style="{width: '50%', height: '570px', margin: 'auto', border: '3px solid green'}">
      <div id="playerScore" v-bind:style="{float: 'left', border: '3px solid green', width: '18%', height: '80px'}">
        <p v-bind:style="{fontFamily: 'Avenir, Helvetica, Arial, sans-serif'}">
          Player Score
        </p>
        <p>{{ playerScore }}</p>
      </div>
      <div id="aiScore" v-bind:style="{float: 'right', border: '3px solid green', width: '18%', height: '80px'}">
        <p v-bind:style="{fontFamily: 'Avenir, Helvetica, Arial, sans-serif'}">
          AI Score
        </p>
        <p>{{ aiScore }}</p>
      </div>
      <p id="aiScreenTitle" v-bind:style="{fontsize: '8'}">AI CHOICE</p>
      <div id="aiScreen" v-bind:style="{border: '3px solid green', height: '35%', width: '25%', marginLeft: '37%'}">
        <img id="aiScreenImg" v-bind:src="getImgUrl" v-bind:style="{height: '100%', width: '100%'}" alt="AI CHOICE IMG">
      </div>
      <img id="versusSymbol" src="./assets/versus.png" v-bind:style="{width: '75px', height: '75px', marginTop: '5px'}">
      <div id="playerScreen"
           v-bind:style="{textAlign: 'center', border: '3px solid green', height: '200px', width: '99%', marginTop: '20px', marginLeft: '1px'}">
        <button id="btnRock" v-bind:style="{height: '100%', width: '30%', position:'relative', float:'left'}"
                v-on:click="play('rock', $event)">
          <img src="./assets/player/rock.png" v-bind:style="{height: '100%', width: '100%'}">
        </button>
        <button id="btnPaper" v-bind:style="{height: '100%', width: '30%', position:'relative'}"
                v-on:click="play('paper', $event)">
          <img src="./assets/player/paper.png" v-bind:style="{height: '100%', width: '100%'}">
        </button>
        <button id="btnScissors" v-bind:style="{height: '100%', width: '30%', position:'relative', float:'right'}"
                v-on:click="play('scissors', $event)">
          <img src="./assets/player/scissors.png" v-bind:style="{height: '100%', width: '100%'}">
        </button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      playerChoice: '',
      aiChoice: '',
      playerScore: 0,
      aiScore: 0,
      aiImg: 'question',
      resultsMap: {
        't': "tie",
        'p': "player",
        'a': "ai"
      },
    }
  },
  computed: {
    getImgUrl() {
      const images = require.context('./assets/ai/', false, /\.png$/)
      return images('./' + this.aiImg + ".png");
    },
    getResultsMatrix() {
      return [
        ['t', 'p', 'a'],
        ['a', 't', 'p'],
        ['p', 'a', 't'],
      ];
    }
  },
  methods: {
    play: function (optionName, event) {
      let choiceStack = ['rock', 'paper', 'scissors'];

      event.preventDefault();
      this.playerChoice = optionName;
      console.log('Player chooses: ' + this.playerChoice);
      this.aiChoice = this.chooseAi();
      console.log('AI chooses: ' + this.aiChoice);
      this.aiImg = choiceStack[this.aiChoice];
      let playerChoiceIndex = choiceStack.indexOf(this.playerChoice);

      const results = this.getResultsMatrix;
      let playerResult = results[this.aiChoice][playerChoiceIndex];
      const resultMap = this.resultsMap;
      let winner = resultMap[playerResult];
      console.log('Winner is: ' + winner)
      this.incrementWinnerScore(winner);

    },
    chooseAi: () => Math.floor(Math.random() * 3),
    incrementWinnerScore: function (winner) {
      switch (winner) {
        case "player":
          this.playerScore++;
          break;
        case "ai":
          this.aiScore++;
          break;
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
