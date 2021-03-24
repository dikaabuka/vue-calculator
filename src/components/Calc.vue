<template>
  <div id="app">
    <div id="calcContainer">
      <div class="calcDisplay">
        <div class="output">{{ total }}</div>
      </div>
      <div id="calcButtons">
        <button class="digit-7" @click="digitHandler(7)">7</button>
        <button class="digit-8" @click="digitHandler(8)">8</button>
        <button class="digit-9" @click="digitHandler(9)">9</button>
        <button class="op-add" @click="digitHandler('+')" :disabled="total === ''">+</button>
        <button class="digit-4" @click="digitHandler(4)">4</button>
        <button class="digit-5" @click="digitHandler(5)">5</button>
        <button class="digit-6" @click="digitHandler(6)">6</button>
        <button class="op-sub" @click="digitHandler('-')">-</button>
        <button class="digit-1" @click="digitHandler(1)">1</button>
        <button class="digit-2" @click="digitHandler(2)">2</button>
        <button class="digit-3" @click="digitHandler(3)">3</button>
        <button class="op-mul" @click="digitHandler('*')" :disabled="total === ''">*</button>
        <button class="digit-0" @click="digitHandler(0)">0</button>
        <button class="op-div" @click="digitHandler('/')" :disabled="total === ''">/</button>
        <button class="clear" @click="clear()">c</button>
        <button class="eq" @click="equal()" :disabled="total === ''">=</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Calculator',
  data() {
    return {
      total: '',
      digits: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'],
      operations: ['*', '+', '-', '/'],
      isCalculated: false,
    };
  },
  methods: {
    digitHandler(num) {
      if (num !== '') {

        let secondLast = null;
        let last = null;

        if (this.total === '') {
          if (num === '*' || num === '+' || num === '/' || num === '') {
            this.clear();
            return;
          }

        }

        if (this.total !== '' && this.total.length > 1) {
          let last = this.total.slice(-1);
          if (num === '-' && last === '-') {
            return;
          }
        }

        if (this.total !== '' && this.total.length === 1) {
          let last = this.total;
          if (num === '-' && last === '-') {
            return;
          }
        }

        if (this.total !== '' && this.total.length >= 2) {
          secondLast = this.total.charAt(this.total.length - 2);
          last = this.total.slice(-1);

          if (this.operations.includes(secondLast) && last === '0' && String(num) !== '-') {
            this.total = this.total.slice(0, -1)
          }

          if (this.operations.includes(String(num)) && this.operations.includes(String(last))) {

            if (last === '*' && num === '-') {
              this.total = JSON.parse(JSON.stringify(this.total));
            } else if (last === '/' && num === '-') {
              this.total = JSON.parse(JSON.stringify(this.total));
            } else if (last === '/' && num === '/') {
              this.total = this.total.slice(0, -1)
            } else {
              this.total = this.total.slice(0, -1)
            }


          }


        }

        if (this.digits.includes(String(num)) && this.isCalculated) {
          this.clear();
          this.isCalculated = false;
        }

        if (this.operations.includes(String(num)) && this.isCalculated) {
          this.isCalculated = false;
        }
        return this.total = String(this.total).charAt(0) === '0' ?
            parseInt(this.total += String(num)) :
            this.total = this.total += String(num);
      }
    },
    clear() {
      return this.total = '';
    },
    equal() {
      let equal = this.total;
      let result = null;

      if (equal !== '' && equal.length > 0) {
        if (this.operations.includes(String(equal.slice(-1)))) {
          return;
        }
      }

      if (this.operations.includes(String(equal.slice(-1)))) {
        this.clear();
        return;
      }

      if (equal === 0) {
        this.clear();
      } else {
        result = Math.round(this.total = eval(equal));
        if (result === Infinity || result === -Infinity || isNaN(result)) {
          this.clear();
          return;
        }
        this.total = parseInt(String(result), 10
        );
        this.isCalculated = true;
        return result;
      }
    }
  }
};
</script>
<style scoped>
html {
  overflow-x: hidden;
}

body {
  font-family: 'Open Sans', sans-serif;
  overflow-x: hidden;
}

#app {
  width: 95%;
  display: flex;
}

#calcContainer {
  background-color: #f7f7f7;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 16px;
  padding: 1.7em 1em 1em 1em;
  margin: 40px auto auto auto;
  width: 292px;
  clear: both;
  display: table;
  vertical-align: middle;
  box-shadow: 0 2px rgba(0, 0, 0, 0.1);
}

.output {
  width: 89%;
  height: 70px;
  display: block;
  background: white;
  border-radius: 3px;
  margin: 0 5px 16px 5px;
  padding: 10px;
  font-size: 20px;
  text-align: left;
  border: 1px solid transparent;
  border-top: none;
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, .39), 0 -1px 1px #FFF, 0 1px 0 #FFF;


}

#calcButtons div, #calcButtons button {
  cursor: pointer;
  width: 21.58%;
  height: 36px;
  font-weight: bold;
  text-align: center;
  line-height: 2em;
  border-radius: 3px;
  margin: 0 5px 11px 5px;
  float: left;
  transition: all 0.2s ease;
  border: 1px solid black;
  box-shadow: 0 0 0.2rem 0 rgba(0, 0, 0, 0.2);
}

#calcButtons button {
  background: lightsalmon;
  color: white;
  box-shadow: 0 0.2rem 0.2rem rgba(0, 0, 0, 0.3);
}

#calcButtons button:hover {
  background: #cb8264;
  cursor: pointer;
}

#calcButtons div:hover {
  background-color: #D6D6D6;
}

#calcButtons .clear {
  background-color: crimson;
  color: white;
}

#calcButtons .clear:hover {
  background-color: #96142f !important;
  cursor: pointer;
}

#calcButtons .eq {
  background-color: #3d6666;
  color: white;
}

#calcButtons .eq:hover {
  background-color: darkslategray;
  cursor: pointer !important;
}

#calcButtons .digit-0,
#calcButtons .digit-1,
#calcButtons .digit-2,
#calcButtons .digit-3,
#calcButtons .digit-4,
#calcButtons .digit-5,
#calcButtons .digit-6,
#calcButtons .digit-7,
#calcButtons .digit-8,
#calcButtons .digit-9 {
  background-color: #758da2;
  color: white;
}

#calcButtons .digit-0:hover,
#calcButtons .digit-1:hover,
#calcButtons .digit-2:hover,
#calcButtons .digit-3:hover,
#calcButtons .digit-4:hover,
#calcButtons .digit-5:hover,
#calcButtons .digit-6:hover,
#calcButtons .digit-7:hover,
#calcButtons .digit-8:hover,
#calcButtons .digit-9:hover {
  background-color: #536575;
  cursor: pointer !important;
}


@media only screen and (max-width: 600px) {

  #calcContainer {
    background-color: #f7f7f7;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 16px;
    padding: 1.7em 1em 1em 1em;
    margin: auto;
    width: 100%;
    clear: both;
    display: table;
    vertical-align: middle;
    box-shadow: 0 2px rgba(0, 0, 0, 0.1);
  }

  #app {
    width: 95%;
    display: flex;
  }
}

@media only screen and (max-width: 321px) {

  #calcContainer {
    background-color: #f7f7f7;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 16px;
    padding: 1.7em 1em 1em 1em;
    margin: auto;
    width: 100%;
    clear: both;
    display: table;
    vertical-align: middle;
    box-shadow: 0 2px rgba(0, 0, 0, 0.1);
  }

  #calcButtons div, #calcButtons button {
    cursor: pointer;
    width: 21.4%;
    height: 36px;
    font-weight: bold;
    text-align: center;
    line-height: 2em;
    border-radius: 3px;
    margin: 0 4.5px 11px 4.5px;
    float: left;
    transition: all 0.2s ease;
    border: 1px solid black;
    box-shadow: 0 0 0.2rem 0 rgba(0, 0, 0, 0.2);
  }

  #app {
    width: 100%;
    display: flex;
  }
}
</style>