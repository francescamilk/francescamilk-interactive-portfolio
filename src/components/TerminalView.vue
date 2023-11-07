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
        <div id="output" v-for="line in output" :key="line">
            <p class="txt">{{ line }}</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            agency: null,
            input: null,
            output: null,
            commands: {}
        }
    },
    methods: {
        getAgency() {
            const urlParams = new URLSearchParams(window.location.search)
            return urlParams.get('a')
        },
        fillCommands () {
            import('@/data/commands.json')
            .then(commandsData => {
                this.commands = commandsData
            })
        },
        getInput(event) {
            if (event.key === 'Enter') {
                this.input = event.target.value.toUpperCase()
                this.computeOutput()
            }
        },
        computeOutput() {
            this.output = this.commands[this.input]
        }
    },
    mounted() {
        this.agency = this.getAgency()
        this.fillCommands()
    }
}
</script>

<style scoped lang="scss">
@import '@/assets/config/variables.scss';

#terminal {
    width: 100%;
    height: 100%;
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