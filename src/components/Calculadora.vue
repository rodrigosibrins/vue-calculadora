<template>
  <div class="calculadora">
    <div class="display">{{ actual || "0" }}</div>
    <div @click="limpiar" class="btn">C</div>
    <div @click="signo" class="btn">+/-</div>
    <div @click="porcentaje" class="btn">%</div>
    <div @click="dividir" class="btn operador">/</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="multiplicar" class="btn operador">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="restar" class="btn operador">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="sumar" class="btn operador">+</div>
    <div @click="append('0')" class="btn cero">0</div>
    <div @click="punto" class="btn">.</div>
    <div @click="igual" class="btn operador">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previo: null,
      actual: "0",
      operador: null,
      operadorActivado: false,
    };
  },
  methods: {
    limpiar() {
      this.actual = "";
    },
    signo() {
      this.actual =
        this.actual.charAt(0) === "-"
          ? this.actual.slice(1)
          : `-${this.actual}`;
    },
    porcentaje() {
      this.actual = `${parseFloat(this.actual) / 100}`;
    },
    append(numero) {
      if (this.operadorActivado) {
        this.actual = "";
        this.operadorActivado = false;
      }
      this.actual = `${this.actual}${numero}`;
    },
    punto() {
      if (this.actual.indexOf(".") === -1) {
        this.append(".");
      }
    },
    setPrevio() {
      this.previo = this.actual;
      this.operadorActivado = true;
    },
    dividir() {
      this.operador = (a, b) => a / b;
      this.setPrevio();
    },
    multiplicar() {
      this.operador = (a, b) => a * b;
      this.setPrevio();
    },
    restar() {
      this.operador = (a, b) => a - b;
      this.setPrevio();
    },
    sumar() {
      this.operador = (a, b) => a + b;
      this.setPrevio();
    },
    igual() {
      this.actual = `${this.operador(
        parseFloat(this.actual),
        parseFloat(this.previo)
      )}`;
      this.previo = null;
    },
  },
};
</script>

<style>
.calculadora {
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: #456268;
  color: white;
}

.cero {
  grid-column: 1 / 3;
}

.btn {
  background-color: #d0e8f2;
  border: 1px solid #fcf8ec;
}

.operador {
  background-color: #79a3b1;
  color: white;
}
</style>
