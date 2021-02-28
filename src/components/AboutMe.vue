<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      It`s ped project based on
      <a href="//www.youtube.com/watch?v=0MEpPU3rWCk&list=PLvTBThJr861yMBhpKafII3HZLAYujuNWw" target="_blank" rel="noopener">series of lessons</a> 
      by JavaScript.Ninja.
    </p>
    <h3>Created by iBan Makarenko</h3>
    <ul>
      <li><a href="//www.linkedin.com/in/ivan-makarenko-050a924a/" target="_blank" rel="noopener">LinkedIn</a></li>
      <li><a href="//github.com/IvanMakarenko" target="_blank" rel="noopener">GitHub</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'AboutMe',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

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
