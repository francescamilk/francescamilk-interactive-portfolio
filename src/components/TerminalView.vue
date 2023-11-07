<template>
    <div id="frame">
        <div id="terminal">
            <section id="terminalHeader">
                <h1 class="txt header">Francesca Milk's Interactive Portfolio</h1>
                <p class="txt header">Type HELP to see the full list of commands.</p>
            </section>
            <section id="previous" v-for="io in previous" :key="io">
                <div class="prompt">
                    <p class="txt">RT C:\Users\{{ agency }}> </p>
                    <p class="txt command">{{ io.input }}</p>
                </div>
                <div class="output" v-for="line in io.output" :key="line">
                    <p :class="{ 
                        'txt': true, 
                        'error': line.includes('recognised'),
                        'success': line.includes('successfully') }"
                        >
                        {{ line ? line : '\u00A0' }}
                    </p>
                </div>
            </section>
            <section id="current">
                <div class="prompt">
                    <p class="txt">RT C:\Users\{{ agency }}> </p>
                    <input type="text" class="txt"
                    v-model="currentInput"
                    @keydown="getInput"
                    ref="inputElement"
                    >
                </div>
            </section>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            agency: null,
            currentInput: '',
            arrowCounter: 0,
            previous: [],
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
                if (this.currentInput.toUpperCase() === 'CLEAR') {
                    this.clearPrevious()
                } else if (this.currentInput.toUpperCase() === 'CV') {
                    const props = {
                        title: 'FSantoriello_FullStack_WebDeveloper.png', 
                        src: require('@/assets/files/FSantoriello_FullStack_WebDeveloper.png') 
                    }
                    this.downloadWithAxios(props.src, props.title)
                    this.processInput()
                } else {
                    this.processInput()
                }
            } if (event.key === 'ArrowUp' && this. isSafeToArrow()) {
                this.currentInput = this.previous[this.arrowCounter].input
                this.increaseArrowCounter()
            } 
        },
        computeOutput() {
            const output = this.commands[this.currentInput.toUpperCase()]
            const error = [
            `FATAL; The term or expression '${this.currentInput}' is not recognised. `,
            'Type HELP for the full list of recognised commands.'
            ]
            
            const inputOutput = {
                input: this.currentInput,
                output: output ? output : error
            }
            this.previous.push(inputOutput)
            this.resetArrowCounter()
            this.$nextTick(() => {
                this.scrollToBottom()
            })        
        },
        triggerFileDownload(response, title) {
            const url = window.URL.createObjectURL(new Blob([response.data]))
            const link = document.createElement('a')
            link.href = url
            link.setAttribute('download', title)
            document.body.appendChild(link)
            link.click()
        },
        downloadWithAxios(url, title) {
            axios({
                method: 'get',
                url: url,
                responseType: 'arraybuffer',
            })
            .then((response) => {
                this.triggerFileDownload(response, title)
            })
            .catch(() => console.log('Error during download'))
        },
        scrollToBottom() {
            const terminal = this.$el.querySelector('#terminal')
            terminal.scrollIntoView({ block: 'end' })
        },
        focusInput() {
            this.$refs.inputElement.focus()
        },
        resetArrowCounter() {
            this.arrowCounter = this.previous.length - 1
        },
        increaseArrowCounter() {
            this.arrowCounter -= 1
        },
        isSafeToArrow() {
            return this.previous.length > 0 && this.arrowCounter >= 0
        },
        clearPrevious() {
            this.previous.splice(0, this.previous.length)
            this.currentInput = ''
        },
        processInput() {
            this.computeOutput()
            this.currentInput = ''
            this.resetArrowCounter()
        }
    },
    mounted() {
        this.agency = this.getAgency()
        this.fillCommands()
        document.addEventListener('click', this.focusInput)
    }
}
</script>

<style scoped lang="scss">
@import '@/assets/config/variables.scss';

#frame {
    width: 100%;
    height: 100%;
    overflow-x: hidden;
    overflow-y: scroll;
}

#terminal {
    padding: 2rem;
}

#terminalHeader,
#previous {
    margin-bottom: 1.5rem;
}
.prompt {
    display: flex;
    
    input,
    input:focus,
    .command {
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

.error {
    color: $errorColor;
    font-weight: 300;
}

.success {
    color: $successColor;
    font-weight: 300;
}
</style>