<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length && index <= 9"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
          <div v-if="index > 9">
            <h4>You completed answering all questions.</h4>
            <h6>Your result</h6>
            <p>
              <b>{{numCorrect}}/{{numTotal}}</b>
            </p>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) this.numCorrect++;
      this.numTotal++;
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "GET"
    })
      .then(res => res.json())
      .then(responseData => {
        this.questions = responseData.results;
      })
      .catch(err => console.log(err));
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
