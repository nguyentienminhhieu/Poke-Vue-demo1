<template>
<div>
  <main-screen v-if="statusMatch == 'default'" @onStart="onHandleBeforeStart($event)" />
  <interact-screen 
  v-if="statusMatch == 'match'" 
  :cardsContext="settings.cardsContext"
  @onFinish="onGetResult"
  />
  <result-screen v-if="statusMatch === 'result'" :timber="timer" @onStartAgain="statusMatch = 'default'"/>
  <copyright/>
</div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import {shuffled} from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue"
import Copyright from "./components/CoppyRightScreen.vue"
export default {
  name: "App",
    components: {
      MainScreen,
      InteractScreen,
      ResultScreen,
      Copyright,
    },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
    }
  },
  methods: {
    onHandleBeforeStart(config)
  {
    console.log("Running", config);
    this.settings.totalOfBlocks = config.totalOfBlocks;
    
    const firstCards = Array.from({length: this.settings.totalOfBlocks / 2}, 
    (_, i) => i+1
    );
    const secondCards = [...firstCards];
    const cards = [...firstCards, ...secondCards];
    console.log(cards);
    this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
    console.log(this.settings.cardsContext);

    this.settings.startedAt = new Date().getTime();



    // data ready
    this.statusMatch = "match";
  },

  onGetResult() {
    // get timer
    this.timer = new Date().getTime() - this.settings.startedAt;

    // switch to result component
    this.statusMatch = " result"
  }
   },
};
</script>

<style>
</style>
