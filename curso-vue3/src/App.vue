<template>
  <div class="pt-5 mb-5">
    <div class="confirmation-message" v-if="showConfirmation">
      <div class="alert alert-success alert-dismissible fade show" role="alert">
        Enquete criada com sucesso!
      </div>
    </div>
    <div class="p-2 conteudo">
      <nav aria-label="breadcrumb">
        <ol class="breadcrumb">
          <li class="breadcrumb-item" aria-current="page">Home</li>
          <li class="breadcrumb-item active" aria-current="page">Criar enquete</li>
        </ol>
      </nav>
      <form @submit.prevent="createSurvey">
        <div v-if="surveyId == null">
          <h4>Criar Enquete</h4>
          <div class="card shadow-sm">
            <div class="card-body">
              <div class="col-md-8 mb-3">
                <h6>Título</h6>
                <input type="text" class="form-control" v-model="surveyTitle" required>
              </div>
              <div class="col-md-8 mb-3">
                <h6>Descrição</h6>
                <textarea class="form-control" v-model="surveyDescription" required> </textarea>
              </div>
              <div class="col-md-8 mb-3">
                <h6>Imagem</h6>
                <input type="text" class="form-control" v-model="surveyUrlImage">
              </div>
              <div class="card-body">
                <div class="row">
                  <div class="col-md-2 mb-3">
                    <h6>Data de Início</h6>
                    <input type="date" class="form-control" v-model="surveyStartDate" required>
                  </div>
                  <div class="col-md-2 mb-3">
                    <h6>Data de Fim</h6>
                    <input type="date" class="form-control" v-model="surveyEndDate" required>
                  </div>
                </div>
                <div class="d-flex justify-content-between">
                  <button type="submit" class="btn btn-dark ml-auto">Cadastrar enquete</button>
                </div>
              </div>
            </div>
          </div>
        </div>
          <h4>Perguntas</h4>
          <div class="card shadow-sm">
            <div class="card-body">
              <div class="row">
                <h6 class="form-label me-2" maxlength="100">Título</h6>
                <div class="d-flex align-items-center mb-2">
                  <input type="text" class="form-control me-2" v-model="fixedQuestion.title">
                </div>
                <h6 class="form-label me-2">Descrição</h6>
                <div class="d-flex align-items-center mb-2">
                  <textarea type="text" class="form-control" v-model="fixedQuestion.description"></textarea>
                </div>
                <div class="col-6">
                  <h6 class="form-label me-2">Mensagem de resposta correta</h6>
                  <div class="d-flex align-items-center mb-2">
                    <input type="text" class="form-control" v-model="fixedQuestion.correctAnswer">
                  </div>
                </div>
                <div class="col-6">
                  <h6 class="form-label me-2">Mensagem de resposta incorreta</h6>
                  <div class="d-flex align-items-center mb-2">
                    <input type="text" class="form-control" v-model="fixedQuestion.wrongAnswer">
                  </div>
                </div>
                <div class="col-6">
                  <h6 class="form-label me-2">Ícone de resposta correta</h6>
                  <div class="d-flex align-items-center mb-2">
                    <input type="text" class="form-control" v-model="fixedQuestion.correctAnswerUrlImage">
                  </div>
                </div>
                <div class="col-6">
                  <h6 class="form-label me-2">Ícone de resposta incorreta</h6>
                  <div class="d-flex align-items-center mb-2">
                    <input type="text" class="form-control" v-model="fixedQuestion.wrongAnswerUrlImage">
                  </div>
                </div>
                <div class="d-flex justify-content-between">
                  <button type="button" class="btn btn-dark mb-2 ml-auto" @click="addQuestion">Adicionar
                    pergunta</button>
                </div>
              </div>
              <div class="row">
                <div class="col-12 ">
                  <table class="table table-bordered">
                    <thead>
                      <tr>
                        <th>Perguntas</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(question, qIndex) in questions" :key="qIndex">
                        <td>
                          <div class="d-flex justify-content-between align-items-center clickable" @click="toggleOptions(qIndex)">
                            <div>
                              <label>Pergunta: {{ question.title }}</label>
                            </div>
                            <div class="d-flex">
                              <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" fill="currentColor"
                                :class="{ 'bi-chevron-down': question.showOptions, 'bi-chevron-right': !question.showOptions }"
                                viewBox="0 0 16 16">
                                <path fill-rule="evenodd"
                                  d="M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z" />
                              </svg>
                            </div>
                          </div>
                          <div class="row d-flex" v-if="question.showOptions">
                            <div class="col-12">
                              <h6 class="form-label me-2" maxlength="100">Alternativa</h6>
                              <div class="d-flex align-items-center mb-2">
                                <input type="text" class="form-control ms-2" v-model="fixedOption.title" @click.stop>
                              </div>
                              <h6 class="form-label me-2">Descrição</h6>
                              <div class="d-flex align-items-center mb-2">
                                <textarea type="text" class="form-control" v-model="fixedOption.description"
                                  @click.stop></textarea>
                              </div>
                              <input class="form-check-input ml-1" type="checkbox" v-model="fixedOption.isCorrect"
                                @click.stop>
                              <label class="form-check-label ml-4">Correta</label>
                              <div class="d-flex justify-content-between">
                                <button type="button" class="btn btn-dark mb-2 mr-2 ml-auto"
                                  @click.stop="addOption(qIndex, question.id)">Adicionar Resposta</button>
                              </div>
                              <div v-for="(option, oIndex) in question.options" :key="oIndex"
                                class="d-flex align-items-center mb-1">
                                <input type="text" class="form-control me-2" v-model="option.title" @click.stop>
                                <button type="button" class="btn btn-outline-danger p-1 ms-2"
                                  @click.stop="deleteOption(option.id, qIndex)">
                                  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                    class="bi bi-trash" viewBox="0 0 16 16">
                                    <path
                                      d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0z" />
                                    <path
                                      d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4zM2.5 3h11V2h-11z" />
                                  </svg>
                                </button>
                              </div>
                              <div class="d-flex justify-content-between">
                                <button type="button" class="btn btn-danger mb-2 mr-2 ml-auto"
                                  @click.stop="deleteQuestion(question.id)">Remover Pergunta</button>
                              </div>
                            </div>
                          </div>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>


export default {
  components: {},
  data() {
    return {
      surveyTitle: '',
      surveyDescription: '',
      surveyStartDate: '',
      surveyEndDate: '',
      surveyUrlImage: '',
      surveyId: null,
      showConfirmation: false,
      fixedQuestion: {
        title: '',
        description: '',
        correctAnswer: '',
        wrongAnswer: '',
        correctAnswerUrlImage: '',
        wrongAnswerUrlImage: '',
        options: [],
      },
      fixedOption: {
        title: '',
        description: '',
        isCorrect: false,
      },
      questions: [
      ]
    };
  },
  methods: {
    createSurvey() {
      const surveyData = {
        title: this.surveyTitle,
        description: this.surveyDescription,
        startDate: this.surveyStartDate,
        endDate: this.surveyEndDate,
        urlImage: this.surveyUrlImage
      }
      createSurvey(surveyData)
        .then((response) => {
          if (!response.success) {
            console.log(response.message)
          } else {
            this.surveyId = response.data.id;
            this.showConfirmation = true;
            setTimeout(() => {
              this.showConfirmation = false;
            }, 3000);
          }
        })
    },
    addQuestion() {
      const questionData = {
        ...this.fixedQuestion,
        options: [],
      };
      addQuestion(questionData, this.surveyId)
        .then((response) => {
          if (response.success) {
            this.getQuestion();
          } else {
            console.log(response.message)
          }
        })
      this.fixedQuestion = {
        title: '',
        description: '',
        correctAnswer: '',
        wrongAnswer: '',
        correctAnswerUrlImage: '',
        wrongAnswerUrlImage: '',
        options: [],
      };
    },
    getQuestion() {
      getQuestion(this.surveyId)
        .then((response) => {
          if (response.success) {
            this.questions = response.data.map(question => ({
              ...question,
              options: question.options || [],
              showOptions: false
            }));
          } else {
            console.log(response.message);
          }
        })
        .catch((error) => {
          console.log(error);
        })
    },
    addOption(qIndex, questionId) {
      const optionData = {
        ...this.fixedOption,
        isAnswerUser: false,
      };
      addOption(optionData, questionId)
        .then((response) => {
          if (response.success) {
            this.getOption(questionId, qIndex);
          } else {
            console.log(response.message)
          }
        })
      this.fixedOption = {
        title: '',
        description: '',
        isCorrect: false,
        isAnswerUser: false,
      };
    },
    getOption(questionId, qIndex) {
      getOption(questionId)
        .then((response) => {
          if (response.success) {
            this.questions[qIndex].options = response.data
          } else {
            console.log(response.message);
          }
        })
        .catch((error) => {
          console.log(error);
        })
    },
    deleteOption(optionId, qIndex) {
      deleteOption(optionId)
        .then((response) => {
          if (response.success) {
            this.getOption(this.questions[qIndex].id, qIndex);
          } else {
            console.log(response.message);
          }
        })
        .catch((error) => {
          console.log(error);
        })
    },
    deleteQuestion(questionId) {
      deleteQuestion(questionId)
        .then((response) => {
          if (response.success) {
            this.getQuestion();
          } else {
            console.log(response.message);
          }
        })
        .catch((error) => {
          console.log(error);
        })
    },
    toggleOptions(qIndex) {
      this.questions.forEach((question, index) => {
        if (qIndex === index) {
          question.showOptions = !question.showOptions;
          this.getOption(question.id, qIndex);
        } else {
          question.showOptions = false;
        }
      });
      this.fixedOption.title = '';
    },
  }
}
</script>

<style scoped>
.pt-45 {
  padding-top: 45px;
}

.alternatives {
  padding-left: 30px;
}

.bi-chevron-down {
  transform: rotate(0deg);
}

.bi-chevron-right {
  transform: rotate(90deg);
}

.btn-outline-secondary {
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
}

.btn-outline-secondary svg {
  width: 16px;
  height: 16px;
}

.table th,
.table td {
  text-align: left;
  padding: 8px;
}

.table-bordered {
  border: 1px solid #dee2e6;
}

.confirmation-message {
  position: fixed;
  top: 5%;
  right: 50%;
}

.clickable {
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.clickable:hover {
  background-color: #f8f9fa;
  /* Cor de fundo mais escura ao passar o mouse */
}
</style>