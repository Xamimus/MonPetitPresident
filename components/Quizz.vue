<template>
  <div class="container-quizz">
    <div
      class="main-quizz"
      v-for="(element, index) in questions[currentQuizzIndex]"
      :key="index"
      v-show="quizz">
      <div class="box-question"></div>
      <p>Question {{ currentQuizzIndex + 1 }}/{{ questions.length }}</p>
      <h3>{{ element.question }}</h3>
      <div class="box-suggestions">
        <ul>
          <li
            v-for="(item, index) in element.reponses"
            :key="index"
            @click="selectResponse(item)"
          >
            {{ item.text }}
            <div></div>
          </li>
          <div class="step-progress" :style="{ width: progress + '%' }"></div>
        </ul>
      </div>
    </div>
    <div class="box-score" v-if="score_show">
      <h2>Voici votre résultat !</h2>
      <div class="results-container">
        <p 
          v-for="(element, index) in displayResult"
          :key="index"
        >
          {{element.name}} est apparu {{element.count}} fois. 
          <span class="result-score">
            {{element.count * (100 / finalResult.length)}} %
          </span>
        </p>
      </div>
      <div class="btn-restart">
        <button @click="restartQuizz">
          Recommencer <i class="fas fa-sync-alt"></i>
        </button>
      </div>
    </div>

    <div class="footer-quizz">
      <div class="box-button" v-if="progress < 100">
        <button
          @click="skipQuestion"
          :style="!next ? 'background-color: rgb(40,162,59)' : ''"
        >
          Skip
        </button>
        <button
          @click="nextQuestion"
          :style="next ? 'background-color: rgb(40,162,59)' : ''"
        >
          Next
        </button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "Quizz",
  data() {
    return {
      questions: [],
      currentQuizzIndex: 0,
      select: false,
      score: 0,
      quizz: true,
      score_show: false,
      next: false,
      progress: 0,
      selectedValue: "",
      finalResult: [],
      displayResult: []
    };
  },
  mounted() {
      this.$fire.firestore
      .collection('questions')
      .doc('Hk7ctzR5hbzEwq2BOoIZ')
      .collection('questions')
      .doc('bVvXtvPSpI4oclpiwqga')
      .get()
      .then((e) => {
        let data = e.data()

        for(let object of Object.entries(data)){
          this.questions.push(object)
        }
      })

      
    },
    updated() {
      const divQuizz = document.querySelector('.main-quizz') ? document.querySelector('.main-quizz').style.display = "none" : null
    },
  methods: {
    selectResponse(e) {
      this.select = true;
      this.next = true;
      var elts = document.getElementsByTagName("li");
      for (let i = 0; i < elts.length; i++) {
        if (e.text === elts[i].innerText) {
          elts[i].style.color = "white";
          elts[i].style.backgroundColor = "green";
        } else {
          elts[i].style.color = "black";
          elts[i].style.backgroundColor = "white";
        }
      }
      this.selectedValue = e.value;
    },
    nextQuestion() {
      if (!this.next) {
        return;
      }
      if(this.selectedValue){
        for(let value of this.selectedValue) {
          this.finalResult.push(value)
        }
      }
      this.selectedValue = ""
      // progress bar
      this.progress = this.progress + 100 / this.questions.length;
      if (this.questions.length - 1 == this.currentQuizzIndex) {
        this.currentQuizzIndex = 0;
        this.score_show = true;
        this.quizz = false;
        this.getResult();
      } else {
        this.currentQuizzIndex++;
        this.select = false;
        this.next = false;
      }

      var elts = document.getElementsByTagName("li");
      for (let i = 0; i < elts.length; i++) {
          elts[i].style.color = 'black',
          elts[i].style.backgroundColor = 'white'
      }
    },
    skipQuestion() {
      if (this.next) {
        return;
      }
      // progress bar
      this.progress = this.progress + 100 / this.questions.length;

      if (this.questions.length - 1 == this.currentQuizzIndex) {
        this.currentQuizzIndex = 0;
        this.score_show = true;
        this.quizz = false;
        this.getResult();
      } else {
        this.currentQuizzIndex++;
      }
      var elts = document.getElementsByTagName("li");
      for (let i = 0; i < elts.length; i++) {
          elts[i].style.color = 'black',
          elts[i].style.backgroundColor = 'white'
      }
    },
    getResult() {
      const possibleAnswers = [
        {slug: "PS", name:"Parti Socialiste", count: 0},
        {slug:"LREM", name:"La République En Marche", count: 0}, 
        {slug:"LFI", name:"La France Insoumise", count: 0}, 
        {slug:"RN", name:"Rassemblement National", count: 0}, 
        {slug:"ELV", name:"Europe écologie Les Verts", count: 0}, 
        {slug:"LR", name:"Les Républicains", count: 0}
      ];

      for(let item of possibleAnswers){
        item.count = this.finalResult.filter(function (value) {return value === item.slug}).length
        this.displayResult.push(item)
      }

      this.displayResult = possibleAnswers;
    },
    restartQuizz() {
      Object.assign(this.$data, this.$options.data()); // reset data
    },
  },
};
</script>


<style scoped>
.container-app {
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
}

.container-quizz {
  display: flex;
  position: relative;
  margin-top: 20px;
  margin-left: 30px;
  margin-right: 30px;
  padding: 10px;
  flex-flow: column;
  text-align: center;
  border-radius: 10px;
  background-color: white;
}

.main-quizz {
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
  margin: auto;
}

.footer-quizz {
  display: flex;
  width: 100%;
  height: 10%;
  justify-content: center;
  justify-content: center;
  border-radius: 0px 0px 10px 10px;
}

.box-question {
  margin-top: 30px;
}

.box-question p {
  margin-top: 20px;
}

h3 {
  margin-bottom: 20px;
}

.box-suggestions {
  display: flex;
  width: 100%;
  margin: auto;
  justify-content: center;
}

ul {
  display: flex;
  width: 80%;
  padding: auto;
  margin: auto;
  flex-flow: column;
}

li {
  list-style-type: none;
  line-height: 2;
  border: 1px solid grey;
  margin-bottom: 20px;
  padding: 5px;
  border-radius: 15px;
  cursor: pointer;
  transition: 0.3s;
}

li > div {
  float: right;
  margin-top: 7px;
  margin-right: 7px;
  color: white;
}

.box-button {
  display: flex;
  width: 100%;
}

.box-button button {
  width: 150px;
  height: 35px;
  outline: none;
  color: rgb(255, 255, 255);
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  border: none;
  background-color: #858484;
}

.box-score {
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
}

.box-score h2 {
  margin-top: 40px;
}

.btn-restart {
  display: flex;
  width: 100%;
  height: auto;
  justify-content: center;
  margin-top: 50px;
}

.btn-restart button {
  width: 250px;
  height: 35px;
  outline: none;
  color: white;
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  background-color: #343a40;
}

.step-progress {
  display: flex;
  height: 5px;
  background: linear-gradient(287.56deg, #ed2939 0%, #002395 100%);
  transition: 0.5s;
  margin-top: 10px;
  margin-bottom: 10px;
}

.result-score{
  font-weight: 800;
}

@media Screen and (max-width: 900px) {
  .container-quizz {
    width: 60%;
  }
}

@media Screen and (max-width: 720px) {
  .container-quizz {
    width: 80%;
  }
  .box-button button {
    width: 100px;
  }
}
</style>
