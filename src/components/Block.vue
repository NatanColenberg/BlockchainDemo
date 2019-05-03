<template>
  <div class="blockDisplay">
    <div class="block" v-bind:style="{ backgroundImage: blockColor}">
      <!-- Header -->
      <block-header 
      class="header"
      v-bind:text="blockText"
      v-bind:state="this.IsHashValid(this.block.hash)"></block-header>

      <!-- Number -->
      <text-field class="textField" title="#" v-bind:text="number" v-bind:readOnly="true"></text-field>

      <!-- Nonce -->
      <text-field class="textField"  title="Nonce" v-bind:text="block.nonce" v-bind:readOnly="true"></text-field>

      <!-- Data -->
      <text-field
        class="textField" 
        title="Data"
        placeholder="Enter Your Data Here..."
        v-bind:text="block.data"
        v-bind:multyLine="true"
        v-on:textChanged="DataChanged"
      ></text-field>

      <!-- Prev -->
      <text-field
        class="textField" 
        title="Prev"
        v-bind:text="block.prev"
        v-bind:multyLine="true"
        v-bind:readOnly="true"
        v-on:textChanged="PrevChanged"
      ></text-field>

      <!-- Hash -->
      <text-field
        class="textField" 
        title="Hash"
        v-bind:text="block.hash"
        v-bind:multyLine="true"
        v-bind:readOnly="true"
      ></text-field>

      <!-- Mine Button -->
      <div>
        <button
          v-on:click="mineButtonClicked()"
          v-bind:disabled="this.IsHashValid(this.block.hash)"
          class="btn btn-primary btn-lg"
          type="button"
        >
          <span
            v-show="isMining"
            class="spinner-border spinner-border-sm"
            role="status"
            aria-hidden="true"
          ></span>
          Mine
        </button>
      </div>
    
    
    
  </div>
  
  </div>
</template>

<script>
//Import Components
import BlockHeader from "./BlockHeader";
import TextField from "./TextField";

// Import Dependencies
import crypto from "crypto";
import axios from 'axios';

export default {
  name: "Block",
  components: {
    "block-header": BlockHeader,
    "text-field": TextField
  },
  props: {
    number: Number,
    block: Object
  },
  created: function() {
    this.block.GetHash = this.GetHash;
    this.block.number = this.number;
    this.block.hash = this.GetHash();
  },
  data: function() {
    return {
      isMining: false
    };
  },
  computed: {
    blockColor: function() {

        if(this.IsHashValid(this.block.hash))
          return "linear-gradient(-45deg, #398235 -1%, #8ab66b 44%, #c9de96 100%)";

        else
          return "linear-gradient(-45deg, #ff7777 -1%, #edca89 49%, #eae9cc 100%)";
    },
    blockText: function() {
      return this.IsHashValid(this.block.hash)
        ? "Valid Block"
        : "Invalid Block";
    }
  },
  methods: {
    DataChanged(newData) {
      this.block.data = newData;
      this.block.hash = this.GetHash();
      this.$emit("checkChain");
    },
    PrevChanged() {
      this.block.hash = this.GetHash();
      this.$emit("checkChain");
    },

    GetHash() {
      return this.CalculateHash(
        this.number,
        this.block.nonce,
        this.block.data,
        this.block.prev
      );
    },

    CalculateHash(...args) {
      // Create concat string
      var stringToHash = "";

      for (var i = 0; i < args.length; i++) {
        stringToHash += String(args[i]);
      }

      // Calculate Hash
      let hash = crypto
        .createHash("sha256")
        .update(stringToHash, "utf8")
        .digest("hex");

      return hash;
    },

    async mineButtonClicked() {
      this.isMining = true;

      let url = `https://7krvzrrppc.execute-api.us-east-1.amazonaws.com/default/mine?
                  number=${this.block.number}&
                  data=${this.block.data}&
                  prev=${this.block.prev}`;

      let response = await axios.get(url);

      if(response.status == 200) {
        this.block.hash = response.data.mineRes.hash;
        this.block.nonce = response.data.mineRes.nonce;
        this.isMining = false;
        this.$emit("checkChain");
      }
    },

    IsHashValid(hash) {
      return hash.startsWith("0000");
    }

  }
};
</script>

<style scoped>
.blockDisplay{
  display: flex;
  flex-basis: 20rem;
  flex-shrink: 0;
  box-shadow: -10px 10px 42px -12px rgba(0,0,0,0.75);
  margin: 2rem 1rem;
}
.block {
  display: flex;
  flex-flow: column;
  border-radius: 10px;
}
.header{
  margin: 10px;
  padding: 5px;
}
.textField{ 
  margin: 0 10px;
}
button {
  margin: 10px;
}
</style>
