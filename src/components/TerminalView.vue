<template>
    <div id="terminal">
        <div id="terminalHeader">
            <h1 class="header txt">Francesca Milk's Playable Portfolio</h1>
            <p class="header txt">Type HELP to see the full list of commands.</p>
        </div>
        <div id="prompt">
            <p class="txt">RT C:\Users\AgencyName></p>
            <input type="text" class="txt">
        </div>
        <div id="output">
            <p class="txt formatted">
                Work:<br>
                Freelance Web Developer...................2021-Present<br>
                Teacher & Batch Manager @ Le Wagon........2021-Present<br>
                Freelance Copywriter (&Wordpress Dev).....2018-2019<br>
                Lead Copywriter & Marketer................2017-2019<br>
                <br>
                Education:<br>
                UX / UI Online Specialization.............California Institue of Arts<br>
                Full-Stack Web Development Bootcamp.......Le Wagon<br>
                Ecommerce & SEO Technical Course..........Euroqualità<br>
                Graphics & Advertising....................High School Diploma<br>
            </p>
        </div>
    </div>
</template>

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