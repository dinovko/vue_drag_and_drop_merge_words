<template>
  <div id="app">
    <main class="flexbox">
      <Board id="board-1" :key="rerenderKey">
        <template v-for="(item, index) in wordsArray">
          <Card
            :id="index"
            draggable="true"
            :key="index"
            @onAccept="handleAccept"
          >
            {{ item }}
          </Card>
        </template>
      </Board>
    </main>
    <div class="input-wrapper">
      <input
        type="text"
        class="inputbox"
        key="input_text"
        multiple
        v-model="inputText"
        @blur="blur"
      />
      <button @click="reset">очистить</button>
    </div>
  </div>
</template>

<script>
import Board from "./components/Board.vue";
import Card from "./components/Card.vue";
export default {
  name: "App",
  components: { Board, Card },
  data() {
    return {
      inputText: "",
      wordsArray: [],
      rerenderKey: 0,
    };
  },
  methods: {
    reset() {
      this.inputText = "";
      this.wordsArray = [];
      this.rerenderKey = -1;
    },
    blur() {
      this.wordsArray = this.inputText.split(" ");
      this.$forceUpdate();
    },
    handleAccept(v) {
      let newArray = this.wordsArray;
      if (v.id < v.appendId) {
        newArray[v.id] = newArray[v.id] + " " + newArray[v.appendId];
      } else {
        newArray[v.id] = newArray[v.appendId] + " " + newArray[v.id];
      }
      newArray.splice(v.appendId, 1);
      this.wordsArray = newArray;
      this.rerenderKey++;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #f3f3f3;
}

.flexbox {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 768px;
  height: 50vh;
  overflow: hidden;
  margin: 0 auto;
  padding: 15px;
  transition: ease-in-out;
  transition-duration: 1ms;
}

.flexbox .board {
  display: flex;
  flex-direction: row;
  width: 100%;
  /* max-width: 300px; */
  background-color: #313131;
  padding: 15px;
  transition-duration: 200ms;
}

.input-wrapper {
  display: flex;
  justify-content: space-between;
  width: 90%;
  height: 20vh;
  overflow: hidden;
  margin: 15px auto;
  padding: 15px;
  background-color: #313131;
}

.input-wrapper > input {
  color: blue;
  padding: 15px;
  font-size: 2em;
  width: 100%;
}

.flexbox .board .card {
  padding: 1em;
  background-color: #f3f3f3;
  cursor: pointer;
  margin-bottom: 15px;
  transition-duration: 200ms;
}

.card {
  max-height: 3em;
  border: 1px solid grey;
  margin: 0px 2px;
}
.card:hover {
  color: white;
  background-color: blueviolet !important;
  padding: 20px !important;
}
</style>
