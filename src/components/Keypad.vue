<template>
    <div class="keypad">
        <button type="button" class="operator" @click="click('operator', '+')">+</button>
        <button type="button" class="operator" @click="click('operator', '-')">-</button>
        <button type="button" class="operator" @click="click('operator', '*')">&times;</button>
        <button type="button" class="operator" @click="click('operator', '/')">&divide;</button>

        <button type="button" class="digit" @click="click('digit', '7')">7</button>
        <button type="button" class="digit" @click="click('digit', '8')">8</button>
        <button type="button" class="digit" @click="click('digit', '9')">9</button>

        <button type="button" class="digit" @click="click('digit', '4')">4</button>
        <button type="button" class="digit" @click="click('digit', '5')">5</button>
        <button type="button" class="digit" @click="click('digit', '6')">6</button>

        <button type="button" class="digit" @click="click('digit', '1')">1</button>
        <button type="button" class="digit" @click="click('digit', '2')">2</button>
        <button type="button" class="digit" @click="click('digit', '3')">3</button>

        <button type="button" class="digit" @click="click('digit', '0')">0</button>
        <button type="button" class="decimal" @click="click('decimal')">.</button>
        <button type="button" class="clear" @click="click('clear')">AC</button>
        <button type="button" class="equal" @click="click('equal')">=</button>
    </div>
</template>

<script>
export default {
  name: 'Keypad',
  methods: {
    click(group, btn) {
      this.$emit('btn-clicked', group, btn);
    },
  },
};
</script>

<style scoped>

    .keypad{
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-gap: 5px;
        padding: 10px;
    }
    button {
        height: 60px;
        #background-color: #fff;
        border-radius: 3px;
        border: 1px solid #c4c4c4;
        font-size: 2rem;
        color: #333;
        background: transparent linear-gradient(to bottom, transparent, transparent 50%,
                rgba(0, 0, 0, .04));
        box-shadow: inset 0 0 0 1px rgba(255, 255, 255, .05),
        inset 0 1px 0 0 rgba(255, 255, 255, .45),
        inset 0 -1px 0 0 rgba(255,255,255,.15), 0 1px 0 0 rgba(255,255,255,.15);
        text-shadow: 0 1px rgba(255,255,255,.4);
    }
    button:hover{
        background-color: #eaeaea;
    }
    .operator {
        color: #337cac;
    }
    clear {
        background-color: #f0595f;
        border-color: #b0353a;
        color: #fff;
    }
    clear:hover{
        background-color: #f17377;
    }
    .equal {
        background-color: #2e86c0;
        border-color: #337cac;
        color: #fff;
        height: 100%;
        grid-area: 2 / 4 / 6 / 5;
    }
    .equal:hover{
        background-color: #4e9ed4;
    }
</style>
