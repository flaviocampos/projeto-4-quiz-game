  <template>
  <div>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <template v-for="answer in this.answers" :key="answer">
        <input type="radio" name="options" value="answer" />
        <label v-html="answer"> </label><br />
      </template>
    </template>

    <button class="send" type="button">Send</button>
  </div>
</template>

//https://opentdb.com/api.php?amount=1&category=18
//https://opentdb.com/api.php?amount=10&category=21
<script>
const api = "https://opentdb.com/api.php?amount=1&category=21";
export default {
  name: "App",
  components: {},
  data() {
    return {
      question: undefined,
      incorrect_answers: undefined,
      correct_answer: undefined,
    };
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrect_answers));
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correct_answer
      );
      return answers;
    },
  },
  created() {
    this.axios.get(api).then((response) => {
      const registro = response.data.results[0];

      this.question = registro.question;
      this.incorrect_answers = registro.incorrect_answers;
      this.correct_answer = registro.correct_answer;
    });
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
