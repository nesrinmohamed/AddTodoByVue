<template>
  <div class="home">
    <img src="../assets/logo.png"  />
    <TodoList />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import TodoList from "@/components/TodoList.vue";
export default {
  name: "HomeView",
  components: {
    HelloWorld,
    TodoList
},
};
</script>
<style>
.home{

}
img{
width: 10%;
}
</style>