<template>
    <TerminalView />
</template>

<script>
import TerminalView from './components/TerminalView.vue'

export default {
    components: {
        TerminalView
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@200;400;600;700&display=swap');

body {
    margin: 0;
    padding: 0;
    overflow-x: hidden;
}

#app {
    width: 100vw;
    height: 100vh;
    min-width: 100vw;
    min-height: 100vh;
    font-family: 'Roboto Mono', monospace;
    background: #222333;
    color: #EDF4F4;
    padding: 2rem;
}
</style>
