<template>
    <div id="history">
        <pre v-for="(line, index) in history" v-bind:key="`line-${index}`">{{line}}</pre>
    </div>
</template>

<script>
export default {
  name: 'History',
  props: { history: Array },
  updated() {
    // scroll history down
    this.$el.scrollTop = this.$el.scrollHeight;
  },
};
</script>

<style scoped>
#history {
    max-height: 50px;
    overflow-y: scroll;
    text-align: right;
    padding: 0 10px;
}
</style>
