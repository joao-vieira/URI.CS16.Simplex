<template>
  <div id="app">
    <Header title="SimplexSolver" subtitle="Solucionador de problemas de programação matemática utilizando o método simplex"></Header>
    <div id="main">
      <Form @generate-solution="generateSolution" />

      <Alert v-if="methodUsed" :method="`Método utilizado: ${methodUsed}!`" description="Abaixo você irá encontrar a solução detalhada deste problema." />
      <Solution @reset="resetApp" v-if="methodUsed" :method="methodUsed" :data="dataToCalculate" />
    </div>
  </div>
</template>


<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";

import Header from "./components/Header";
import Form from "./components/Form";
import Alert from "./components/Alert";
import Solution from "./components/Solution";

export default {
  name: "app",
  components: {
    Header, Form, Alert, Solution
  },
  data: function() {
    return {
      methodUsed: "",
      dataToCalculate: {},
    };
  },
  methods: {
    generateSolution(dataToCalculate) {
      const mustContinue = this.validateData(dataToCalculate);
      if (mustContinue) {
        this.methodUsed = this.defineMethod(dataToCalculate);
        this.dataToCalculate = dataToCalculate;
      }
    },
    validateData(data) {
      const { min, independentTerms, ofCoefficients, trCoefficients } = data;
      if (independentTerms.length < min || ofCoefficients.length < min || trCoefficients.length < min) {
        this.showToasted("É necessário que ao menos duas variáveis sejam utilizadas!", "error", "times-circle");
        return false;
      }
      return true;
    },
    defineMethod(data) {
      const { signals } = data;
      const diffLessOrEqual = signals.filter(signal => signal != "<=");

      if (diffLessOrEqual.length == 0) {
        return "Simplex Padrão";
      } else {
        return "M Grande";
      }
    },
    resetApp() {
      this.methodUsed = "";
      this.dataToCalculate = {};
    },
    showToasted(text, type = "info", icon = "fa-exclamation-circle") {
      this.$toasted.show(text, {
        type: type,
        icon: icon
      });
    },
  }
};
</script>


<style>
</style>
