<template>
  <div class="block">
    <h1 class="header">
      {{text}}
      <i class="fa fa-check" v-show="state" style="font-size:2rem"></i>
      <i class="fa fa-close" v-show="!state" style="font-size:2rem"></i>
    </h1>
  </div>
</template>

<script>
export default {
  name: "Header",
  components: {

  },
  props: {
    text: String,
    state: Boolean
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
  .header{
    font-size: 2rem;
    cursor: context-menu;
  }
  .fa-check{
    color: rgb(55, 116, 55);
  }
  .fa-close{
    color: rgb(184, 27, 27);
  }
</style>
