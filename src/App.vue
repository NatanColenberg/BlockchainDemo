<template>
  <div>
    <h1 class="headline">
      Blockchain Demo
      <img class="logo vueLogo" src="../public/vueLogo.png">
      <img class="logo nodeLogo" src="../public/nodeLogo.png">
      <img class="logo awsLogo" src="../public/awsLambdaLogo.png">
    </h1>
    
    <div class="app" v-on:wheel="ScrollFromVarticalToHorizontal">
      <template v-for="(b, index) in blocks">
        <Block
          class="block"
          v-bind:key="index"
          v-bind:number="index"
          v-bind:block="b"
          v-on:checkChain="CheckChain"
        />
        <i class="arrow fa fa-arrow-right" v-bind:key="'arrow_'+index" style="font-size:3rem"></i>
      </template>

      <div class="addNew" v-on:click="AddNewBlock()">
        <div style="margin:2rem">Add New Block</div>
        <div><i class="fa fa-plus-square-o" style="font-size:5rem"></i></div>
      </div>
    </div>
  </div>
</template>

<script>
import Block from "./components/Block";
import tippy from 'tippy.js'

export default {
  name: "app",
  components: {
    Block
  },
  created: function() {

    this.AddBlock({
      nonce: 20986,
      data: "Alice gave to Bob 10$",
      prev: "0000000000000000000000000000000000000000000000000000000000000000",
      hash: "",
      isValid: true
    });

    this.AddBlock({
      nonce: 44876,
      data: "Bob gave to Sally 2$ and Bob gave to Jim 3$",
      prev: "0000f8b362b15f45089da388736e9acc437f904b0647b6a6b140737b3b0a9118",
      hash: "",
      isValid: true
    });

    this.AddBlock({
      nonce: 63829,
      data: "",
      prev: "000008d23f31ca6fe84a3d77859f11e47017687c8dde86ae76b4eeec919b7686",
      hash: "",
      isValid: true
    });

  },
  mounted: function(){

    tippy('.vueLogo', {
      content: "Vue JS",
    });
    tippy('.nodeLogo', {
      content: "Node JS",
    });
    tippy('.awsLogo', {
      content: "AWS Lambda",
    });

  },
  data: function() {
    return {
      blocks: [],
    };
  },
  methods: {
    AddNewBlock() {
      this.blocks.push({
        number: this.blocks.length,
        nonce: 0,
        data: "",
        prev: this.blocks[this.blocks.length - 1].hash,
        hash: "",
        isValid: true
      });
      window.scrollBy(0,500);
    },
    AddBlock(block) {
      this.blocks.push(block);
    },
    CheckChain() {
      for (var i = 1; i < this.blocks.length; i++) {
        this.blocks[i].prev = this.blocks[i - 1].hash;
      }
    },
    ScrollFromVarticalToHorizontal(event) {
      const conent = document.querySelector('.app');
      conent.scrollLeft -= event.wheelDeltaY;
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Alfa+Slab+One');
.headline{
  padding: 1rem;
  margin-left: 1rem;
  font-family: 'Alfa Slab One', cursive;
  font-size: 4rem;
  text-decoration: underline solid gray;
  cursor: context-menu;
}
.headline::first-letter {
    color: rgb(141, 0, 0)
}
.logo{
  width: 70px;
  margin: 10px;
  padding: 10px;
  border: 5px solid rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 40%;
}
.app {
  display: flex;
  align-items: center;
  overflow-x: auto;
  
  padding: 20px;
}
.arrow{
  margin: 10px;
}
.addNew{
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  font-weight: bold;
  color: rgba(0, 0, 0, 0.4);
  width: 20rem;
  height: 30rem;
  flex-shrink: 0;
  background-color: rgba(0, 0, 0, 0.1);
  cursor: pointer;
  border: 4px dashed rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  margin-right: 20px;
}
/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.05); 
}
 
/* Handle */
::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.3);  
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: rgba(0, 0, 0, 0.5); 
}
</style>
