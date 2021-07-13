<template>
  <div id="app">
    <!--<img alt="Vue logo" src="./assets/logo.png">-->
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
//import PersonListCard from "@/components/PersonListCard";
export default {
  name: 'App',
  components: {
    HelloWorld
  },
  mounted() {
    const { NODE_ENV } = process.env;
    document.documentElement.className = NODE_ENV;
  }
}
</script>

<style>
#app {
  font-family: "楷体", "楷体_GB2312", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin: 0 0;
  padding: 0 0;

}
::-webkit-scrollbar {
  width: 5px;
  height: 14px;
}

::-webkit-scrollbar-thumb {
  background-color: #b6b6b6;
}
</style>
