<template v-if="">
  <section class="score">
    Player <span>{{ winCount }}</span> X <span>{{ loseCount }}</span> Computer
  </section>
</template>

<script>
export default {
  props: ["winCount", "loseCount"],
};
</script>
<style lang="scss">
section.score {
  border-bottom: 1px solid #000;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 8px;
    margin: 4px;
    font-weight: bold;
    border: 1px solid #000;
  }
}
</style>
