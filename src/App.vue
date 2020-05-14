<template>
  <div id="app">
    <h1>Записная книжка</h1>
    <nodes_book></nodes_book>
  </div>
</template>

<script>
import nodes_book from "@/components/nodesBook";
export default {
  name: 'App',
  components:{
    nodes_book: nodes_book
  }
}
</script>
<style>
#app {
  width: 100%;
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
  background-color: rgb(37, 47, 90);
    color: white;
}
</style>

