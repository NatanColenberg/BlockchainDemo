<template>
  <div class="block">
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="basic-addon3">{{title}}</span>
      </div>
      <!-- Single Line -->
      <input
        v-show="!multyLine"
        v-bind:readonly="readOnly"
        v-model="text"
        v-bind:placeholder="placeholder"
        type="text"
        class="form-control"
        aria-describedby="basic-addon3"
        spellcheck="false">

      <!-- Multy Line -->
      <textarea
        v-show="multyLine"
        v-bind:readonly="readOnly"
        v-model="text"
        v-bind:placeholder="placeholder"
        class="form-control"
        aria-describedby="basic-addon3"
        rows="3"
        spellcheck="false"
      ></textarea>

    </div>
  </div>
</template>

<script>
export default {
  name: "TextField",
  components: {

  },
  props: {
      title: String,
      multyLine: Boolean,
      readOnly: Boolean,
      text: {
        type: [String, Number],
      },
      placeholder: String
  },
  watch:{
    text(){
      this.$emit('textChanged', this.text);
    }
  },
  data: function() {
    return {

    };
  },
  computed: {
  },
  methods: {
    
  }
};
</script>

<style scoped>
  .input-group-text{
    width:70px;
  }
  .noscroll {
  overflow: hidden;
}
</style>
