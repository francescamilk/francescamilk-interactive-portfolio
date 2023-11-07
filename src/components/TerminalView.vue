<template>
    <div id="frame">
        <div id="terminal">
            <section id="terminalHeader">
                <h1 class="txt header">{{ header }}</h1>
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
            header: 'Francesca Milk\'s Interactive Portfolio',
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
                const specialInputs = ['CLEAR', 'CV', 'SHARE', 'HEADER']
                const sanitisedInput = this.currentInput.toUpperCase().split(' ')[0]

                specialInputs.includes(sanitisedInput) ? this.processSpecialInputs() : this.processInput()
            } if (event.key === 'ArrowUp' && this. isSafeToArrow()) {
                this.currentInput = this.previous[this.arrowCounter].input
                this.increaseArrowCounter()
            } 
        },
        processInput(shouldItCompute) {
            shouldItCompute ? this.computeOutput() : 
            this.currentInput = ''
            this.resetArrowCounter()
        },
        processSpecialInputs() {
            const sanitisedInput = this.currentInput.toUpperCase()
            const fileProps = {
                title: 'FSantoriello_FullStack_WebDeveloper.png',
                src: require('@/assets/files/FSantoriello_FullStack_WebDeveloper.png')
            }

            if (sanitisedInput === 'CLEAR') {
                this.clearPrevious()
            } else if (sanitisedInput === 'CV') {
                this.downloadWithAxios(fileProps.src, fileProps.title)
                this.processInput(true)
            } else if (sanitisedInput === 'SHARE') {
                this.copyURLToClipboard()
                this.processInput(true)
            } else if (sanitisedInput.includes('HEADER')) {
                this.setCustomHeader()
                this.processInput(false)
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
        clearPrevious() {
            this.previous.splice(0, this.previous.length)
            this.currentInput = ''
        },
        copyURLToClipboard() {
            navigator.clipboard.writeText(window.location.href)
        },
        setCustomHeader() {
            this.header = this.currentInput
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
        scrollToBottom() {
            const terminal = this.$el.querySelector('#terminal')
            terminal.scrollIntoView({ block: 'end' })
        },
        focusInput() {
            this.$refs.inputElement.focus()
        },
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
        padding-left: .6rem;
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