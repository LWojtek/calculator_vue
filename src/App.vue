<template>
  <div id="app">
      <div class="calc">
        <div class="calc__top">
          <h1>calc</h1>
          <div></div>
        </div>
        <div class="calc__screen">
          {{current || '0'}}
        </div>
        <div class="calc__keypad">
          <div @click="append('7')" class="btn">7</div>
          <div @click="append('8')" class="btn">8</div>
          <div @click="append('9')" class="btn">9</div>
          <div @click="backspace" class="btn delete">DEL</div>
          <div @click="append('4')" class="btn">4</div>
          <div @click="append('5')" class="btn">5</div>
          <div @click="append('6')" class="btn">6</div>
          <div @click="add" class="btn">+</div>
          <div @click="append('1')" class="btn">1</div>
          <div @click="append('2')" class="btn">2</div>
          <div @click="append('3')" class="btn">3</div>
          <div @click="substract" class="btn">-</div>
          <div @click="dot" class="btn">.</div>
          <div @click="append('0')" class="btn">0</div>
          <div @click="divide" class="btn">/</div>
          <div @click="times" class="btn">x</div>
          <div @click="reset" class="btn reset">RESET</div>
          <div @click="equal" class="btn equal">=</div>
        </div>
      </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked:false,
    }
  },
  methods: {
    backspace(){
      var result = this.current.slice(0, -1);
      this.current = result;
    },
    append(num){
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${num}`;
    },
    reset(){
      this.current = '';
    },
    dot(){
      if (this.current.indexOf('.') === -1) {
        this.append('.')
      }
    },
    setPrev(){
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide(){
      this.operator = (a,b) => b/a;
      this.setPrev();
    },
    times(){
      this.operator = (a,b) => b*a;
      this.setPrev();
    },
    add(){
      this.operator = (a,b) => b+a;
      this.setPrev();
    },
    substract(){
      this.operator = (a,b) => b-a;
      this.setPrev();
    },
    equal(){
      if (this.current.length > 12) {
        this.current = parseFloat(`${this.operator(
          parseFloat(this.current),
          parseFloat(this.previous)
      )}`).toFixed(2);
      } else {
        this.current = `${this.operator(
          parseFloat(this.current),
          parseFloat(this.previous)
        )}`;
      }
      this.previous = null;
    }

  }
}
</script>

<style lang="scss">

@import '@/assets/scss/global.scss';
@import '@/assets/scss/variables.scss';

#app {
  display: flex;
  justify-content: center;
  align-items: center;

  background-color: $main-bg;
  width: 100vw;
  height: 100vh;
}

.calc {
  width: 45rem;
  height: 64rem;
  // background: #000;
  position: relative;

  display: flex;
  flex-direction: column;
}

.calc__top {
  display: flex;
  flex-basis: 15%;
  justify-content: space-between;
  align-items: center;
  padding: 3rem 0;

  h1,
  div {
    font-size: 3rem;
    color: $white;
  }

}

.calc__screen {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  flex-basis: 15%;
  font-size: 3.2rem;
  padding: 2rem;
  width: 100%;
  height: 13rem;
  background-color: $screen-bg;
  border-radius: 1rem;
  margin-bottom: 3rem;
  color: $white;
}

.calc__keypad {
  flex-basis: 70%;
  background-color: $keypad-bg;
  padding: 2rem;
   border-radius: 1rem;

  display: grid;
  grid-gap: 2rem;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}

.btn {
  display: flex;
  justify-content: center;
  align-items: center;

  font-size: 2rem;
  background-color: $white;
  border-radius: 1rem;
  cursor: pointer;

  box-shadow: 0 4px 0px hsl(0, 0%, 46%);
  transition: box-shadow 150ms;

  &:active {
    box-shadow: none;
  }
}

.reset {
  grid-column: 1/3;
}

.reset,
.delete {
  background: $key-bg;
  color: $white;
  box-shadow: 0 4px 0px $key-shadow;
}

.equal {
  background: $equal-key-bg;
  color: $white;
  box-shadow: 0 4px 0px $equal-key-shadow;
  grid-column: 3/5;
}



</style>
