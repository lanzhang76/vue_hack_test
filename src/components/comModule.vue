<template>
  <div class="comedian-module" draggable="true">
    <h3 ref="comedian_code">{{comedian_name}}</h3>
    <div class="output_box">
      <p>
        <span v-for="({input, output},key) in conversationChain" :key="key">
          <span>
            {{" " + input}}
            <span style="color:#9770EA">{{output}}</span>
          </span>
        </span>
      </p>
    </div>
    <!-- <p for v-for="({battleOut},key) in battleCompilation" :key="key">
      <span>{{battleOut}}</span>
    </p>-->
    <textarea
      ref="textinput"
      v-on:keyup="init_byKey"
      cols="40"
      rows="3"
      placeholder="Start typing here"
    ></textarea>
    <button v-on:click="init">Write</button>
    <sourceCaption v-show="captionWanted" v-bind:comcap="this.comedian_name"></sourceCaption>
  </div>
</template>


<script>
import { bus } from "../main";
import { generateEnd } from "./Markov.js";
import sourceCaption from "./sourceCaption.vue";
import aziz from "raw-loader!../assets/aziz.txt";
import jerry from "raw-loader!../assets/jerry.txt";
import amy from "raw-loader!../assets/amy.txt";
import bill from "raw-loader!../assets/bill.txt";
import ali from "raw-loader!../assets/ali.txt";
import louis from "raw-loader!../assets/louis.txt";

export default {
  name: "comModule",
  props: {
    comedian_name: {
      type: String,
      default: ""
    },
    captionWanted: {
      type: Boolean,
      default: true
    }
  },
  components: {
    sourceCaption
  },
  data() {
    return {
      // battleCompilation: [{ battleOut: "some" }],
      text_label: this.comedian_name,
      show_captions: this.captionWanted,
      conversationChain: [
        {
          input: "",
          output: ""
        }
      ]
    };
  },
  watch: {
    // This would be called anytime the value of title changes
    toWatchSomeValue(newValue, oldValue) {
      //do stuff with newValue and oldValue
    }
  },
  methods: {
    nameCheck: function(label) {
      if (label == "Aziz Ansari") {
        return aziz;
      } else if (label == "Jerry Seinfeld") {
        return jerry;
      } else if (label == "Amy Schumer") {
        return amy;
      } else if (label == "Bill Maher") {
        return bill;
      } else if (label == "Ali Wong") {
        return ali;
      } else if (label == "Louis C.K.") {
        return louis;
      }
    },
    init: function() {
      let current_sentence = this.$refs.textinput.value;
      let selected_corpus = this.nameCheck(this.text_label);
      let sentence = generateEnd(current_sentence, selected_corpus);
      this.$refs.textinput.value = "";
      this.appendToConversation(sentence[0], sentence[1]);
    },
    appendToConversation(input1, input2) {
      this.conversationChain.push({
        input: input1,
        output: input2.replace(input1, "")
      });
    },
    init_byKey: function(e) {
      if (e.keyCode === 13) {
        this.init();
      }
    },
    battleToText: function(battleIn) {
      let current_battleIn = battleIn;
      let selected_corpus = this.nameCheck(this.text_label);
      let sentence = generateEnd(current_battleIn, selected_corpus);
      this.appendToConversation(sentence[0], sentence[1]);
    }
  },
  created() {
    bus.$on("battleF", this.battleToText);
  }
};
</script>


<style scoped>
@import url(https://fonts.googleapis.com/css?family=Roboto+Mono&display=swap);
.comedian-module {
  display: inline-block;
  vertical-align: top;
  text-align: left;
  padding: 2vw;
  background-color: white;
  border: 2px solid black;
  border-radius: 5px;
  width: 300px;
  margin: 0 auto;
}

textarea {
  font-family: "Roboto Mono", monospace;
  font-size: 0.8em;
  outline: none;
  padding: 5px;
  box-sizing: border-box;
  width: 100%;
  font-size: inherit;
}

.output_box {
  margin-top: 1vh;
  margin-bottom: 1vh;

  /* overflow-y: scroll; */
}

.output_box span {
  font-size: 15px;
}
</style>