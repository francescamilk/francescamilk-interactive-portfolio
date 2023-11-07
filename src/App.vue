<template>
    <TerminalView />
</template>

<script>
import TerminalView from '@/components/TerminalView'

export default {
    components: {
        TerminalView
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@200;400;600;700&display=swap');
@import '@/assets/config/variables.scss';

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
    background: $backgroundColor;
    color: $fontColor;
    padding: 2rem;
}
</style>
