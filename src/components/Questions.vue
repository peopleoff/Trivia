<template>
  <v-container grid-list-md text-xs-center v-if="questions">
    <v-layout row wrap>
      <v-flex xs12 v-for="(question, index) in questionArray" :key="index">
        <v-card class="mx-auto" color="#26c6da" dark>
          <v-card-title>
            <v-icon left>question_answer</v-icon>
            <span class="title font-weight-light">{{question.category}}</span>
          </v-card-title>
          <v-card-text class="font-weight-bold">
            <p v-html="question.question"></p>
            <v-radio-group column color="red" v-model="pickedAnswers[index]" @click="selectAnswer(question, pickedAnswers[index])">
              <v-radio
                v-for="answer in question.all_answers"
                :key="answer.id"
                :label="decodeHtml(answer.answer)"
                :value="decodeHtml(answer.answer)"
                :color="colorAnswer(answer.correct_answer)"
              ></v-radio>
            </v-radio-group>
          </v-card-text>
          <v-card-actions class="mx-4"></v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: "Questions",
  props: ["questions", "gameType"],
  data() {
    return {
      pickedAnswers: []
    };
  },
  mounted() {},
  methods: {
    randomizeQuestions(correct_answer, incorrect_answers) {
      let answers = [
        {
          correct_answer: true,
          answer: correct_answer
        }
      ];

      incorrect_answers.forEach(element => {
        let object = {
          correct_answer: false,
          answer: element
        };
        answers.push(object);
      });
      return this.shuffle(answers);
    },
    selectAnswer(question, answer){
      console.log(question);
      console.log(answer);

    },
    colorAnswer(correct_answer) {
      if (this.gameType === 1) {
        if (correct_answer) return "green";
        return "red";
      }
    },
    decodeHtml(html) {
      var txt = document.createElement("textarea");
      txt.innerHTML = html;
      return txt.value;
    },
    shuffle(a) {
      var j, x, i;
      for (i = a.length - 1; i > 0; i--) {
        j = Math.floor(Math.random() * (i + 1));
        x = a[i];
        a[i] = a[j];
        a[j] = x;
      }
      return a;
    }
  },
  computed: {
    questionArray: function(){
      let newArray = [];
      this.questions.forEach(element => {
        element.all_answers = this.randomizeQuestions(element.correct_answer, element.incorrect_answers)
        newArray.push(element);
      });
      return newArray
    }
  }
};
</script>
