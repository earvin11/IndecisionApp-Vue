<template>
  <img v-if="image" v-bind:src="image" alt="">
  <div class="bg-dark"></div>

  <div class="indecision-container">

      <input 
      type="text"
      v-model="question" 
      placeholder="Hazme una pregunta">

      <p>Recuerda terminar con un signo de interrogacion (?)</p>

      <div v-if="isValidQuestion">
          <h2>{{ question }}</h2>
          <h1>{{ answer }}</h1>
      </div>

  </div>


</template>

<script>
export default {
    data() {
        return {
            question: '',
            answer: null,
            image: null,
            isValidQuestion: false
        }
    },
    methods: {
        async getAnswer() {

            this.answer = 'Pensando...'

            const { answer, image } = await fetch('https://yesno.wtf/api').then( resp => resp.json())

            this.answer = (answer === 'yes') ? 'Si!'
                : (answer === 'no') ? 'No!'
                : 'Tal Vez';
            this.image = image;

        }
    },
    // Watch contiene los observadores de las propiedades reactivas
    watch: {
        // Aqui podemos realizar acciones dependiendo del valor que vaya tomando this.question
        question( value, oldValue ) {
            // Cuando tipee sigue siendo una question invalida
            this.isValidQuestion = false;
            // Si el valor de question no incluye el '?' return y salte de la funcion
            if( !value.includes('?') ) return
            //Cuando lo incluya haz la peticion HTTP
            // Realizar peticion HTTP llamando el metodo getAnswer, y ahora si es valida la question
            this.isValidQuestion = true;
            this.getAnswer()
        }
    }
}
</script>

<style scoped>
    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: rgba(0, 0, 0, 0.4);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 20px;
        margin-top: 0px;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }
</style>