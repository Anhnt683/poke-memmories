<template>
  <main-screen v-if="statusMatch == 'default'" @onStart="onHandleBeforStart($event)"></main-screen>
  <interact-screen v-if="statusMatch == 'match'" :cardContext="settings.cardContext" @onFinish="OnGetResult"></interact-screen>
  <result-screen v-if="statusMatch == 'result'" :timer="timer" @onStartAgain="statusMatch == 'default'"></result-screen>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from './components/ResultScreen.vue';
import { shuffled } from "./utils/array";
export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardContext: [],
        startAt: null,
      },
      statusMatch: "default",
      timer: 0
    }
  },
 
  methods: {
    onHandleBeforStart(config) {
      console.log("Running onHandleBeforStart", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from({ length: this.settings.totalOfBlocks / 2 }, (_, i) => i + 1);
      const secondCards = [...firstCards];

      const cards = [...firstCards, ...secondCards];

      this.settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));
      this.settings.startAt = new Date().getTime();


      console.log(this.settings.cardContext);

      this.statusMatch = "match";
    },
    OnGetResult() {
      this.timer = new Date().getTime() - this.settings.startAt;

      this.statusMatch = "result";
    }
  }
}
</script>


