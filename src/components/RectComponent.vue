<template>
  <div>
    <div class="rect" @click="filling">{{ inner }}</div>
  </div>
</template>

<script>
import EventBus from "@/eventBus";

export default {
  name: "RectComponent",

  props: ["position"],

  created() {
    EventBus.$on("prevent", () => {
      this.canYouClick = false;
    });
    EventBus.$on("restart", () => {
      this.inner = "";
      this.canYouClick = true;
    });
  },

  methods: {
    filling: function () {
      if (this.canYouClick) {
        this.inner = this.$parent.activePlayer;
        this.canYouClick = false;
        EventBus.$emit("pop", this.position);
      }
    },
  },

  data: function () {
    return {
      inner: "",
      canYouClick: true,
    };
  },
};
</script>

<style lang="css" scoped>
.rect {
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  background: #2c3e50;
  font-family: "Permanent Marker", cursive;
  transition: 0.1s ease-in-out;
  font-size: 68px;
  cursor: pointer;
}
.rect:hover {
  background: #405971;
}
</style>
