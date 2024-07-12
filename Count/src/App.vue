
<script setup>
import {ref} from "vue"
  const count = ref(0);
  console.log(count)
</script>
<template>
  <main>
    <div>
      <h4>                    
        The current count is ....
      </h4>
      <h1>{{count}}</h1>
      <button @click="count--">-</button>
      <button @click="count++">+</button>
    </div>
  </main>
</template> 
<style scoped>
main {
  background-color: rgb(84, 120, 148);
  height: 100vh;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
}

div{
  text-align: center;
}

button{
  font-size: 30px;
  width: 40px;
  height: 40px;
  margin: 10px;
  cursor: pointer;
}
</style>
