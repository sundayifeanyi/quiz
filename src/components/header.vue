<template>
  <div>
    <b-nav class="navbar">
      <b-nav-item disabled>quiz logo</b-nav-item>
      <b-nav-item disabled>counter: {{numCorrect}}/{{total}}</b-nav-item>
    </b-nav>
    <hr>
  </div>
</template>

<script>
export default {
  props:[
     "numCorrect",
       "total"
    
  ]
}
</script>

<style scoped>
/* .navbar{
  padding-bottom: 5px;
  margin-bottom: 3px
} */
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
