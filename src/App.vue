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
body {
    margin: 0;
    padding: 0;
}

#app {
    width: 100vw;
    height: 100vh;
    min-width: 100vw;
    min-height: 100vh;
    max-width: 100vw;
    max-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
}
</style>
