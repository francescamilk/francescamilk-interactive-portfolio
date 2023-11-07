<template>
    <div id="terminal">
        <div id="terminalHeader">
            <h1 class="txt header">Francesca Milk's Interactive Portfolio</h1>
            <p class="txt header">Type HELP to see the full list of commands.</p>
        </div>
        <div id="prompt">
            <p class="txt">RT C:\Users\{{ agency }}> </p>
            <input type="text" class="txt" @keydown.enter="getInput">
        </div>
        <div id="output">
            <p class="txt">{{ output }}</p>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      agency: null,
      input: null,
      output: null
    }
  },
  methods: {
    getAgency() {
      const urlParams = new URLSearchParams(window.location.search)
      return urlParams.get('a')
    },
    getInput(event) {
      if (event.key === 'Enter') {
        this.input = event.target.value;
      }
    }
  },
  mounted() {
    this.agency = this.getAgency()
  }
}
</script>

<style scoped lang="scss">
@import '@/assets/config/variables.scss';

#terminal {
    width: 100vw;
    height: 100vh;
    min-width: 100vw;
    min-height: 100vh;
}

#terminalHeader {
    margin-bottom: 2rem;
}

#prompt {
    display: flex;
    
    input,
    input:focus {
        background: transparent;
        box-shadow: none;
        border: none;
        outline: none;
        padding-left: 1rem;
    }
    
}

.txt {
    margin: 0;
    padding: 0;
    margin-bottom: .2rem;
    font-family: $fontMain;
    color: $fontColor;
    font-size: $fontSize;
}

.header {
    font-size: calc(#{$fontSize} - #{.5 * 0.01} * #{$fontSize});
    font-weight: 200;
}
</style>