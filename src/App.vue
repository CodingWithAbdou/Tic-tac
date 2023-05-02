<template>
  <div id="app">
    <h1>Tic-Tac-Game</h1>
    <div class="info">
      <h2>
        Game : <span class="num">{{ numatches + 1 }}</span>
      </h2>
      <span class="player">X </span> Has
      <span class="num">{{ this.win.X }}</span> Wins
      <span class="space">|</span>
      <span class="player">O </span> Has
      <span class="num">{{ this.win.O }}</span> Wins
    </div>
    <GridComponent />
    <button class="restart" @click="restart()">Restart</button>
  </div>
</template>

<script>
import GridComponent from "@/components/GridComponent.vue";
import EventsBus from "./eventBus";

export default {
  name: "App",

  props: ["whoWin"],

  created() {
    EventsBus.$on("aWinner", (Winner) => this.win[Winner]++);
  },

  methods: {
    restart() {
      EventsBus.$emit("restart");
      this.numatches++;
    },
  },

  data: function () {
    return {
      numatches: 0,
      win: {
        O: 0,
        X: 0,
      },
    };
  },
  components: {
    GridComponent,
  },
};
</script>

<style>
#app {
  font-family: "Dosis", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 400px;
  margin: 0 auto;
}

.restart {
  width: 100%;
  padding: 2rem;
  background: red;
  color: white;
  border: 0;
  font-size: 2em;
  font-weight: bold;
  border-radius: 0 0 50px 50px;
  cursor: pointer;
}
h1 {
  font-size: 3rem;
  margin: 0;
}
h2 {
  margin-block: 0 0.5rem;
  text-decoration: underline;
}
.info {
  padding-block: 0.5em;
  font-size: 1.5em;
}
.player {
  font-family: "Permanent Marker", cursive;
}
.info > .space {
  padding-inline: 1em;
}
.num {
  color: #3085d6;
  font-weight: bold;
}
</style>
