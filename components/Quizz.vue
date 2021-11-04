<template>
  <div class="container-quizz">
    <div class="step-progress" :style="{ width: progress + '%' }"></div>
    <div
      class="main-quizz"
      v-for="(element, index) in questions.slice(a, b)"
      :key="index"
      v-show="quizz"
    >
      <div class="box-question"></div>
      <h2>Question{{ b }}/{{ questions.length }}</h2>
      <p>{{ element.question }}</p>
      <div class="box-suggestions">
        <ul>
          <li
            v-for="(item, index) in element.suggestions"
            :key="index"
            @click="selectResponse(item)"
          >
            {{ item.suggestion }}
            <div></div>
          </li>
        </ul>
      </div>
    </div>
    <div class="box-score" v-if="score_show">
      <h2>Votre score est</h2>
      <h2>{{ score }}/{{ questions.length }}</h2>
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
      // Quizz content
      questions: [
        {
          // Q1 - Institution
          question: "La Vème République",
          suggestions: [
            {
              suggestion:
                "Elle doit être amendée, notamment en instaurant le scrutin proportionnel aux élections législatives.",
              value: "PS",
            },
            {
              suggestion:
                "Elle doit être remplacée par la VIème République, où seront remis à plat le fonctionnement démocratique dans une assemblée citoyenne constituante.",
              value: "LFI",
            },
            {
              suggestion: "Elle ne nécessite pas d’ajustement.",
              value: ["LR", "LREM", "RN"],
            },
          ],
        },
        {
          // Q2 - Ecologie
          question: "Accord de Paris",
          suggestions: [
            {
              suggestion:
                "Nous devons impérativement le respecter, ce qui implique un changement de course drastique de notre mode de production et des investissements très importants pour combler notre retard.",
              value: "ELV",
            },
            {
              suggestion:
                "Respecter l’accord de Paris passe derrière la croissance et l’économie française, ce qui implique de soigner celle-ci avant le climat.",
              value: "LREM",
            },
            {
              suggestion:
                "Nous devons le respecter en intensifiant nos efforts pour tenir les objectifs sans changer de mode de production, ce qui signifie s’assurer que l’économie et la croissance ne décrochent pas.",
              value: "PS",
            },
            {
              suggestion:
                "Nous devons le respecter en intensifiant nos efforts pour tenir les objectifs sans changement brutal du mode de production.",
              value: "LR",
            },
          ],
        },
        {
          // Q3 - Finances publiques
          question: "ISF (Impôt de Solidarité sur la Fortune)",
          suggestions: [
            {
              suggestion:
                "Le rétablissement de cet impôt seul suffit à faire participer équitablement les français les plus aisés. D’autres mesures complémentaires ne sont pas nécessaires.",
              value: "LREM",
            },
            {
              suggestion:
                "Il ne doit pas être rétabli, mais des mesures ciblant les plus aisés peuvent être envisagées, comme une hausse de l’impôt remplaçant l’ISF : L’ISFI (Impôt sur la Fortune Immobilière).",
              value: "LFI",
            },
            {
              suggestion:
                "La suppression de cet impôt a été un bon signal envoyé aux investisseurs étrangers et sa réhabilitation ferait du mal à l’image et l’économie de la France.",
              value: "LR",
            },
            {
              suggestion:
                "Il doit être rétabli pour aider à relancer l’économie et d’autres mesures doivent être mises en place pour faire participer davantage les classes sociales les plus aisées.",
              value: "PS",
            },
          ],
        },
        {
          // Q4 - Sécurité & Justice
          question:
            "IGPN (Inspection Générale de la Police Nationale), aussi appelée “la Police des Polices”",
          suggestions: [
            {
              suggestion:
                "L’IGPN n’a pas à être mise en cause. Les violences policières n’existent pas, ceux-ci ne font que répliquer à des provocations.",
              value: "RN",
            },
            {
              suggestion:
                "L’IGPN est efficace et travaille loin des médias. Elle prend son temps, mais fonctionne. Elle doit continuer à le faire pour punir les quelques éléments fautifs dans la police.",
              value: "LREM",
            },
            {
              suggestion:
                "L’IGPN a montré qu’elle était défaillante. Elle doit être réformée en profondeur et bénéficier de moyens augmentés pour effectuer sa mission.",
              value: "PS",
            },
            {
              suggestion:
                "Les manifestations des Gilets Jaunes ont montré l’incapacité de l’IGPN d’effectuer son travail. La police ne parvient pas à enquêter sur elle-même, l’IGPN doit donc être dissoute et remplacée par un nouvel organisme indépendant.",
              value: "LFI",
            },
          ],
        },
        {
          // Q5 - Sécurité & Justice
          question: "Projet de loi Confiance dans l’institution judiciaire",
          suggestions: [
            {
              suggestion:
                "Cette loi verse dans la surenchère sécuritaire au lieu de s’attaquer aux vraies causes de l’insécurité. Elle devrait être abrogée.",
              value: "LFI",
            },
            {
              suggestion:
                "Cette loi va dans le bon sens mais reste très insuffisante. Le rétablissement des peines plancher est notamment indispensable.",
              value: "PS",
            },
            {
              suggestion:
                "Cette loi est équilibrée et permet de rapprocher les citoyens de l’institution judiciaire tout en durcissant l’arsenal pénal.",
              value: "LREM",
            },
          ],
        },
        {
          // Q6 - Immigration
          question: "Droit du sol",
          suggestions: [
            {
              suggestion:
                "C’est un droit qui doit être rediscuté et potentiellement réformé.",
              value: "PS",
            },
            {
              suggestion:
                "C’est un droit fondamental, mais il peut être suspendu dans certains cas précis, notamment sur l’île de Mayotte où beaucoup d’immigration illégale a lieu.",
              value: "LR",
            },
            {
              suggestion:
                "C’est un droit qui ne correspond plus aux réalités modernes et doit être abrogé.",
              value: "RN",
            },
            {
              suggestion:
                "C’est un droit fondamental qui ne doit pas être modifié sur l’intégralité du territoire national.",
              value: "LFI",
            },
          ],
        },
        {
          // Q7 - Europe
          question: "Europe",
          suggestions: [
            {
              suggestion:
                "L’intégration Européenne doit être poursuivie avec pour axes principaux la solidarité entre les pays membre après de nombreuses année d’intégrations économiques.",
              value: "PS",
            },
            {
              suggestion:
                "Il doit être refondé en y intégrant plus de démocratie, d’écologie et de solidarité entre les pays membre.",
              value: "ELV",
            },
            {
              suggestion:
                "L’Europe vole le pouvoir d’auto-détermination de la France et sa souveraineté. Nous devons fonder une Europe de pays souverains, une Europe des Nations ou la quitter.",
              value: "LFI",
            },
            {
              suggestion:
                "Il est profondément néolibéral. La France doit affirmer ses positions en Europe en sortant des traités s’ils ne peuvent être retravaillés.",
              value: "LR",
            },
            {
              suggestion:
                "Face aux défis actuels, l’intégration doit rester économique. De plus, l’immigration aux frontières de l’espace Schengen doit être plus contrôlée.",
              value: "RN",
            },
            {
              suggestion:
                "Toutes les formes d’intégration sont capitales pour assurer à l’Europe son statut de puissance mondiale.",
              value: "LREM",
            },
          ],
        },
        {
          // Q8 - Emploi
          question: "Le système de retraite par répartition",
          suggestions: [
            {
              suggestion:
                "Le système doit être gardé en relevant l’âge de départ à 64 ans, les régimes spéciaux doivent être supprimés et le secteur public doit s’aligner sur le privé. De cette manière, les cotisations des français et leurs pensions ne seront pas baissées.",
              value: "LR",
            },
            {
              suggestion:
                "Nous devons le garder et retourner à la retraite à 60 ans, quitte à ce que le système ne s’auto-finance plus seul.",
              value: "LFI",
            },
            {
              suggestion:
                "Nous devons le garder (pas de retraite par point) et maintenir l’âge de départ à la retraite à 62 ans.",
              value: "PS",
            },
            {
              suggestion:
                "Le système doit être gardé et la loi Touraine, déjà en application, doit continuer à être appliquée, amenant progressivement la durée de cotisation de 41.5 ans à 43 ans en 2035.",
              value: "LREM",
            },
          ],
        },
        {
          // Q9 - Emploi
          question: "La durée légale de travail hebdomadaire (35h / semaine)",

          suggestions: [
            {
              suggestion:
                "La loi doit être revue afin d’augmenter le temps de travail pour améliorer l’économie et la compétitivité des entreprises.",
              value: "LREM",
            },
            {
              suggestion:
                "La durée du travail est juste et la législation à ce sujet ne doit pas être modifiée.",
              value: "LR",
            },
            {
              suggestion:
                "Nous devons passer à 32h par semaine afin de créer davantage d’emplois.",
              value: "LFI",
            },
          ],
        },
        {
          // Q10 - Cannabis
          question: "Le cannabis",
          suggestions: [
            {
              suggestion:
                "Le cannabis est une drogue à fort effet psychoactif passée et vendue par des cartels violents, souvent affiliés au terrorisme. Son utilisation, même médicale entraîne des risques sanitaires et éthiques trop importants pour le patient et la société.",
              value: "RN",
            },
            {
              suggestion:
                "Il doit être légalisé afin de pouvoir mettre en place des politiques de prévention et d’aide aux consommateurs en plus de désengorger la police, la justice et de créer des emplois.",
              value: "LFI",
            },
            {
              suggestion:
                "Des tests doivent être menés pour permettre le cannabis thérapeutique accompagné de contrôles et vérifications strictes. La loi ne doit cependant pas bouger en dehors du domaine médical.",
              value: "PS",
            },
            {
              suggestion:
                "Des tests doivent être menés pour permettre au cannabis thérapeutique d’être commercialisé et progressivement assouplir la législation sur le cannabis.",
              value: "LR",
            },
          ],
        },
        {
          // Q11 - Dette
          questions: "La dette française",
          suggestions: [
            {
              suggestion:
                "La dette de tous les pays est abyssale, et différents exemples récents montrent qu’elle n’a pas toujours vocation à être payée. Son augmentation est possible et nécessaire pour la transition écologique.",
              value: "LFI",
            },
            {
              suggestion:
                "La dette, qu’elle soit écologique ou économique, doit être remboursée. Cependant, l’urgence climatique impose de s’endetter pour verdir notre société.",
              value: "PS",
            },
            {
              suggestion:
                "Une dette doit être remboursée, sans quoi personne ne prêtera à la France en cas de coup dur. C’est également un garant de notre indépendance et notre souveraineté. La maîtrise des dépenses et le remboursement de la dette doivent donc être parmis les priorités nationales.",
              value: "LREM",
            },
          ],
        },
        {
          // Q12 - Education
          question: "SNU (Service National Universel)...",
          suggestions: [
            {
              suggestion:
                "Cette mesure doit être transformée en “Service Citoyen” de 9 mois payé au SMIC pour les jeunes de 18 à 25 ans avec passage du permis, examens médicaux et éventuellement une partie maniement des armes.",
              value: "RN",
            },
            {
              suggestion:
                "Cette mesure est un bon compromis entre le service militaire classique et la journée d’appel. Elle va améliorer la cohésion sociale, on doit donc la garder.",
              value: "PS",
            },
            {
              suggestion:
                "Cette mesure n’est pas suffisante. Un retour au Service National Obligatoire de 3 mois minimum doit être entrepris.",
              value: "RN",
            },
            {
              suggestion:
                "Cette mesure est bonne mais va venir s'ajouter aux missions et au coûts de notre armée. Le projet doit être repensé et mieux préparé.",
              value: "LR",
            },
            {
              suggestion:
                "Cette mesure est coûteuse et peu utile. Une amélioration du parcours citoyen de l’école à l’université et du parcours civique est plus adaptée.",
              value: "LFI",
            },
          ],
        },
        {
          // Q13 - Solidarité
          question: "APL (Aides Pour le Logement)...",
          suggestions: [
            {
              suggestion:
                "Les APL doivent être augmentées, notamment pour aider à faire face au COVID.",
              value: "PS",
            },
            {
              suggestion:
                "Les APL ont un niveau satisfaisant et peuvent éventuellement servir de variable d’ajustement pour faire de petites économies.",
              value: "LR",
            },
            {
              suggestion:
                "Avoir un toit est un droit fondamental et les APL doivent être augmentées pour permettre à tout le monde d’avoir accès au logement, ainsi que la suppression du mois de carence.",
              value: "LFI",
            },
            {
              suggestion:
                "Les APL ont un niveau correct et ne doivent pas être spécialement revalorisées.",
              value: "LREM",
            },
          ],
        },
        {
          // Q14 - Santé publique
          question: "AME (Aide Médicale d’État) ...",
          suggestions: [
            {
              suggestion:
                "Le budget de l’AME doit continuer à évoluer pour couvrir les besoins de ses bénéficiaires, mais son obtention doit aller vers plus de restrictions.",
              value: "LR",
            },
            {
              suggestion:
                "Tout doit être mis en place pour faciliter l’accès des bénéficiaires aux soins, et le délais de carence de 3 mois doit être supprimé.",
              value: "LFI",
            },
            {
              suggestion:
                "L’AME incite à l’immigration clandestine pour se faire soigner. Elle doit être supprimée.",
              value: "RN",
            },
            {
              suggestion:
                "L’AME doit être restreinte et recentrée autour des soins urgents.",
              value: "LREM",
            },
            {
              suggestion:
                "Le budget de l’AME doit continuer à évoluer pour couvrir les besoins de ses bénéficiaires.",
              value: "PS",
            },
          ],
        },
        {
          // Q15 - Transport
          question:
            "Les vols en avion à l’intérieur de la France Métropolitaine...",
          suggestions: [
            {
              suggestion:
                "Cette mesure est principalement symbolique et va coûter cher en emplois et économiquement pour un effet moindre : C’est de l’écologie punitive, elle n’aurait pas dû être mise en œuvre.",
              value: ["LR", "LREM"],
            },
            {
              suggestion:
                "La loi a placé le seuil au bon endroit (2h30) sans trop pénaliser la filière aéronautique ou les français.",
              value: "PS",
            },
            {
              suggestion:
                "La loi n’a pas été assez loin. Afin de lutter efficacement contre le réchauffement climatique, ce sont tous les trajets substituables en train en moins de 4h qui devraient être interdits.",
              value: "ELV",
            },
          ],
        },
        {
          // Q16 - Convention Climat
          question: "Concernant la Convention Citoyenne pour le Climat",
          suggestions: [
            {
              suggestion:
                "L’ensemble des propositions de la Convention Citoyenne auraient dû être présentées à l’Assemblée et soumises au vote des parlementaires.",
              value: "ELV",
            },
            {
              suggestion:
                "L’utilisation des jokers par le Président est parfaitement justifiée face à des idées trop extrêmes. L’essentiel des propositions ont été portées au vote, comme convenu.",
              value: "LREM",
            },
            {
              suggestion:
                "Cette Convention n’est qu’un outil de communication déployé par le gouvernement pour mettre en place une écologie punitive. La seule assemblée légitime pour proposer des lois est l’Assemblée Nationale.",
              value: "LFI",
            },
            {
              suggestion:
                "L’intégralité des propositions de la Convention auraient dû être présentées à l’Assemblée, soumises au vote des parlementaires et toutes auraient dû être ratifiées.",
              value: "PS",
            },
          ],
        },
      ],

      a: 0,
      b: 1,
      select: false,
      score: 0,
      quizz: true,
      score_show: false,
      next: false,
      progress: 0,
    };
  },
  methods: {
    selectResponse(e) {
      this.select = true;
      this.next = true;
      if (e.correct) {
        this.score++;
      }
      console.log(e.suggestion)
      var elts = document.getElementsByTagName("li");
      for (let i = 0; i < elts.length; i++) {
        if (e.suggestion === elts[i].innerText) {
          elts[i].style.color = 'white',
          elts[i].style.backgroundColor = 'green'
        } else {
          elts[i].style.color = 'black',
          elts[i].style.backgroundColor = 'white'
        }
      }
    },
    // check(status) {
    //   if (status.correct) {
    //     return "correct";
    //   } else {
    //     return "uncorrect";
    //   }
    // },
    nextQuestion() {
      if (!this.next) {
        return;
      }
      // progress bar
      this.progress = this.progress + 100 / this.questions.length;
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quizz = false;
      } else {
        this.a++;
        this.b++;
        this.select = false;
        this.next = false;
      }
    },
    skipQuestion() {
      if (this.next) {
        return;
      }
      // progress bar
      this.progress = this.progress + 100 / this.questions.length;

      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quizz = false;
      } else {
        this.a++;
        this.b++;
      }
    },
    restartQuizz() {
      Object.assign(this.$data, this.$options.data()); // reset data
    },
  },
};
</script>


<style scoped>
.nextbtn {
  background-color: #080036;
  border-radius: 5px;
  color: #fff;
}
.nexbtn:hover {
  background-color: #fff;
  color: #080036;
  border: 1px solid #080036;
}

.prevbtn {
  background-color: #430000;
  color: white;
}

.prevbtn:hover {
  background-color: #fff;
  color: #430000;
  border: 1px solid #430000;
}

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
  border: 1px solid rgb(133, 133, 133);
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

h2 {
  margin-bottom: 10px;
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
  width: 100%;
  height: 5px;
  background-color: #17b860;
  transition: 0.5s;
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