<template>
  <div class="battle-module">
    <div id="textInput">
      <textarea
        ref="battletextinput"
        v-on:keyup="init_byKey"
        cols="100"
        rows="1"
        placeholder="For starting battle mode and see different outputs, type here."
      ></textarea>
      <button v-on:click="submit_sentence">Write</button>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";

export default {
  name: "battle",
  data() {
    return {};
  },
  methods: {
    init_byKey: function(e) {
      if (e.keyCode === 13) {
        console.log("battle submitted");
        this.submit_sentence();
      }
    },
    submit_sentence: function() {
      let current_sentence = this.$refs.battletextinput.value;
      bus.$emit("battleF", current_sentence.replace("\n", ""));
      this.$refs.battletextinput.value = "";
    }
  }
};
</script>

<style scoped>
@import url(https://fonts.googleapis.com/css?family=Roboto+Mono&display=swap);
.battle-module {
  display: inline-block;
  vertical-align: center;
  text-align: left;
  padding: 10px;
  background-color: black;
  border: 2px solid black;
  color: blueviolet;
  border-radius: 5px;
  width: 800px;
  margin: 0 auto;
}
#textInput {
  width: 600px;
  display: inline-block;
}

#textInput textarea {
  font-family: "Roboto Mono", monospace;
  font-size: 0.7em;
  background: transparent;
  color: white;
  outline: none;
  padding: 3px;
  box-sizing: border-box;
  width: 100%;
}

#textInput button {
  font-size: 0.4em;
}
</style>