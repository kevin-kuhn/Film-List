<template>
  <div class="home">
    <h1>Para assistir mais tarde</h1>
    <Lista/>
  </div>
</template>

<script>
import Lista from "../components/Lista";
export default {
  name: "Home",
  components: {
    Lista
  }
};
</script>

<style lang="less" scoped>
.home{
  width: 70%;
  height: 70%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
