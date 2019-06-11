<template>
  <v-container grid-list-md text-xs-center>
    <v-layout row wrap v-if="questions.length === 0">
      <v-flex xs12>
        <v-text-field label="Number of Questions" v-model="numberOfQuestions"></v-text-field>
      </v-flex>
      <v-flex xs12>
        <v-select
          :items="categorys"
          v-model="categorysSelected"
          :menu-props="{ maxHeight: '400' }"
          label="Question Categorys"
        ></v-select>
      </v-flex>
      <v-flex xs12>
        <v-select
          :items="difficulties"
          v-model="difficultieSelected"
          :menu-props="{ maxHeight: '400' }"
          label="Question Difficulty"
        ></v-select>
      </v-flex>
      <v-flex xs12>
        <v-select
          :items="types"
          v-model="typeSelected"
          :menu-props="{ maxHeight: '400' }"
          label="Question Types"
        ></v-select>
      </v-flex>
      <v-flex xs12>
        <v-select
          :items="gameTypes"
          v-model="gameSelected"
          :menu-props="{ maxHeight: '400' }"
          label="Game Type"
        ></v-select>
      </v-flex>
      <v-btn fab fixed bottom right color="pink" @click="generateApiUri()">
        <v-icon>navigate_next</v-icon>
      </v-btn>
    </v-layout>
    <loading v-bind:loading="loading"/>
    <questions v-bind:questions="questions" v-bind:gameType="gameSelected" v-if="questions.length > 0" />
    <v-btn fab fixed bottom left color="pink" @click="questions = []" v-if="questions.length !== 0">
      <v-icon>navigate_before</v-icon>
    </v-btn>
  </v-container>
</template>

<script>
import loading from "@/components/includes/Loading.vue";
import questions from "@/components/Questions.vue";

export default {
  name: "Home",
  components: {
    loading,
    questions
  },
  data() {
    return {
      loading: false,
      numberOfQuestions: 10,
      categorysSelected: "any",
      categorys: [
        { text: "Any Category", value: "any" },
        { text: "General Knowledge", value: "9" },
        { text: "Entertainment: Books", value: "10" },
        { text: "Entertainment: Film", value: "11" },
        { text: "Entertainment: Music", value: "12" },
        { text: "Entertainment: Musicals & Theatres", value: "13" },
        { text: "Entertainment: Television", value: "14" },
        { text: "Entertainment: Video Games", value: "15" },
        { text: "Entertainment: Board Games", value: "16" },
        { text: "Science & Nature", value: "17" },
        { text: "Science: Computers", value: "18" },
        { text: "Science: Mathematics", value: "19" },
        { text: "Mythology", value: "20" },
        { text: "Sports", value: "21" },
        { text: "Geography", value: "22" },
        { text: "History", value: "23" },
        { text: "Politics", value: "24" },
        { text: "Art", value: "25" },
        { text: "Celebrities", value: "26" },
        { text: "Animals", value: "27" },
        { text: "Vehicles", value: "28" },
        { text: "Entertainment: Comics", value: "29" },
        { text: "Science: Gadgets", value: "30" },
        { text: "Entertainment: Japanese Anime & Manga", value: "31" },
        { text: "Entertainment: Cartoon & Animations", value: "32" }
      ],
      difficultieSelected: "any",
      difficulties: [
        {
          text: "Any Difficulty",
          value: "any"
        },
        {
          text: "Easy",
          value: "easy"
        },
        {
          text: "Medium",
          value: "medium"
        },
        {
          text: "Hard",
          value: "hard"
        }
      ],
      typeSelected: "any",
      types: [
        {
          text: "Any Type",
          value: "any"
        },
        {
          text: "Multiple Choice",
          value: "multiple"
        },
        {
          text: "True / False",
          value: "boolean"
        }
      ],
      gameSelected: 0,
      gameTypes: [
        {
          text: "Solo",
          value: 0
        },
        {
          text: "Multiplayer",
          value: 1
        }
      ],

      questions: []
    };
  },
  mounted() {},
  methods: {
    generateApiUri() {
      let url = new URL("https://opentdb.com/api.php");
      if (this.numberOfQuestions) {
        url.searchParams.append("amount", this.numberOfQuestions);
      }
      if (this.difficultieSelected !== "any") {
        url.searchParams.append("difficulty", this.difficultieSelected);
      }
      if (this.typeSelected !== "any") {
        url.searchParams.append("type", this.typeSelected);
      }
      if (this.categorysSelected !== "any") {
        url.searchParams.append("category", this.categorysSelected);
      }
      this.loading = true;
      this.getQuestions(url.href);
    },
    getQuestions(url) {
      fetch(url)
        .then(res => res.json())
        .then(response => {
          //Verify good response from API
          if (response.response_code === 0) {
            this.questions = [];
            response.results.forEach(element => {
              element.id = Math.random()
                .toString(36)
                .substr(2, 9);
              element.incorrect_answers.forEach(answer => {
                let answers = [
                  {
                    correct_answer: true,
                    answer: element.correct_answer
                  }
                ];
                let object = {
                  correct_answer: false,
                  answer: answer
                };
                answers.push(object);
              });
              this.questions.push(element);
            });
            this.loading = false;
          }
        })
        .catch(error => console.error("Error:", error));
    },
    shuffle(array) {
      var currentIndex = array.length,
        temporaryValue,
        randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    }
  }
};
</script>
