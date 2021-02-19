<template>
  <div id="app" @keydown="keyPressed()">
    <div id="calculator">
      <div id="output">
        {{chars.join('')}}
      </div>
      <div class="contianer">
        <button
        class="Buttons"
        v-for="button in buttons"
        :key="button"
        @click="addChar(button)">{{button}}</button>
      </div>
      <button class="equalButton"
      @click="eval"
      >=</button>
    </div>
  </div>
</template>

<script>

function evaluate(chars) {
  const ops = ["+", "-", "*", "/"]
  let founda = chars.reduce((ace, char, i) => {
    if (ops.includes(char)) {
      ace.push([char, i])
    }
    return ace
  },
  []);
  let pos = founda[0][1]
  if (founda.length > 1) {
    pos = founda[1][1]
  }
  let op = chars[pos]
  let before = parseFloat(chars.slice(0, pos).join(""))
  let after = parseFloat(chars.slice(pos + 1).join(""))
  console.log(before, op, after);
  if (op == "+") {
    return before + after
  }
  if (op == "-") {
    return before - after
  }
  if (op == "*") {
    return before * after
  }
  if (op == "/") {
    return before / after
  }
  return 0;
}

export default {
  name: 'App',
  components: {

  },
  data(){
    return {
      buttons: ["clear", "1", "2", "+", "3", "4", "5", "-", "6",  "7", "8", "*", "9",  "0", ".", "/"],
      chars: [],
      done: false
    }
  },
  methods: {
    addChar: function (char) {
      if (char == "clear") {
        this.chars = []
        return
      }
      const ops = ["+", "-", "*", "/"]
      if (char == "-" && (this.chars.length == 0 || ops.includes(this.chars[this.chars.length - 1]))) {
        this.chars.push(char)
        return
      }
      if (!ops.includes(char)) {
          if (this.done == true) {
            this.chars = []
            this.done = false
          }
          this.chars.push(char)
          return
      }
      let founda = this.chars.filter(c => ops.includes(c));
      if (founda.length == 2 || founda.length == 1 && !ops.includes(this.chars[0])) {
        this.eval()
        this.done = false
      }
      this.chars.push(char)
      this.done = false
    },
    keyPressed: function(event) {
      console.log(event);
    },
    eval: function() {
      let result = evaluate(this.chars)
      this.chars = []
      this.chars.push(result)
      this.done = true
    }
  }
}
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
#calculator{
  display: flex;
  flex-direction: column;
  height:  47%;
  justify-content: space-between;
}
#output{
  width: 208px;
  height: 2rem;
  /*margin-top: -255px;
  margin-right: -208px;*/
  border: 1px solid black;
  max-width: 208px;
  max-height: 2rem;
  overflow:hidden;
  border-radius: 4px;
  text-align: right;
  box-shadow: 0 1px 1px -9px rgba(0, 0, 0, 0.2), 0 1px 8px 2px rgba(0, 0, 0, 0.14), 0 1px 1000px 1px rgba(0, 0, 0, 0.12);
  padding:0.5rem;
  box-sizing: border-box;
  font-size: 1rem;
}

html, body {
  width:100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
.contianer {
  display: grid;
  grid-template: repeat(4, 1fr) / repeat(4, 1fr);
  grid-auto-columns: 1fr;
  grid-auto-rows: 1fr;
  /* This is better for small screens, once min() is better supported */
  /* grid-template-columns: repeat(auto-fill, minmax(min(200px, 100%), 1fr)); */
  grid-gap: 1rem;
  /* This is the standardized property now, but has slightly less support */
  /* gap: 1rem */
  width: 200px;
  height: 200px;
}
.Buttons{
  width: 40px;
  height: 40px;
}
.equalButton{
  height: 40px;
  width: 208px;
  /* margin-bottom: -280px;
  margin-left: -200px; */
}
</style>
