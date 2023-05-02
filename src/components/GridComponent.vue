<template>
  <div>
    <div class="colorStatus" :class="colorStatus">{{ gameStatusMessage }}</div>
    <div class="grid">
      <RectComponent v-for="number in 9" :key="number" :position="number" />
    </div>
  </div>
</template>

<script>
import RectComponent from "./RectComponent.vue";
import EventBus from "@/eventBus";

export default {
  name: "GridComponent",
  created() {
    EventBus.$on("pop", (rectNumber) => {
      this.rects[rectNumber] = this.activePlayer;
      this.numberOfMove++;
      this.gameStatus = this.UpdateStatus();
    });
    EventBus.$on("restart", () => {
      Object.assign(this.$data, this.$options.data());
    });
  },

  methods: {
    isWon() {
      return this.winCanditions.some((candition) => {
        this.counter = 0;
        candition.forEach((key) => {
          if (this.rects[key] === this.activePlayer) {
            this.counter++;
          }
        });
        if (this.counter === 3) return true;
        else return false;
      });
    },
    whenWin() {
      this.gameStatusMessage = `${this.activePlayer} Wins`;
      EventBus.$emit("prevent");
      EventBus.$emit("aWinner", this.activePlayer);
      return "won";
    },
    UpdateStatus() {
      if (this.isWon()) {
        return this.whenWin();
      } else if (this.numberOfMove === 9) {
        this.gameStatusMessage = `No Winner`;
        return "draw";
      }
      this.gameStatusMessage = `${this.activePlayer}'s Turn`;

      this.activePlayer = this.reverseActivePlayer;
      return "turn";
    },
  },
  computed: {
    reverseActivePlayer() {
      if (this.activePlayer == "O") return "X";
      else return "O";
    },
  },
  watch: {
    gameStatus() {
      if (this.gameStatus == "turn") this.colorStatus = "turnColor";
      else if (this.gameStatus == "won") this.colorStatus = "wonColor";
      else this.colorStatus = "drawColor";
    },
  },
  data() {
    return {
      activePlayer: "O",
      gameStatus: "turn",
      gameStatusMessage: "O's turn",
      colorStatus: "turnColor",
      numberOfMove: 0,
      counter: 0,
      winCanditions: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7],
      ],
      rects: {
        1: "",
        2: "",
        3: "",
        4: "",
        5: "",
        6: "",
        7: "",
        8: "",
        9: "",
      },
    };
  },

  components: {
    RectComponent,
  },
};
</script>

<style lang="css" scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(3, 120px);
  gap: 10px;
  background: #34495e;
  padding: 10px;
  box-sizing: border-box;
}

.colorStatus {
  padding: 1.5rem;
  font-size: 2em;
  color: #fff;
  font-weight: 800;
  border-radius: 50px 50px 0 0;
}
.turnColor {
  background: #ffc107;
}
.wonColor {
  background: #8bc34a;
}
.drawColor {
  background: #dededd;
}
</style>
