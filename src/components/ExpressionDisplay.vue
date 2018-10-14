<template>
    <pre class="expression-line">{{expressionDisplay}}</pre>
</template>

<script>

export default {
  name: 'ExpressionDisplay',
  props: {
    expression: Array,
    result: Number,
  },
  computed: {
    expressionDisplay() {
      if (this.expression.length === 0) {
        return '0';
      }
      const expressionStr = this.expression.join(' ');
      // eslint-disable-next-line no-eval
      return `${expressionStr} = ${this.result}`;
    },
  },
};

</script>
<style scoped>
.expression-line{
    text-align: right;
    background-color: antiquewhite;
    padding: 10px;
}
</style>
