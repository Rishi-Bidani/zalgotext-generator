<template>
    <div id="app">
        <h1>Random Diacritics | Zalgotext</h1>
        <main>
            <textarea name="inputtext" cols="30" rows="10" v-model="inputText"></textarea>
            <article class="container">
                <div class="output">
                    {{ output }}
                </div>
                <button class="clipboard" @click="copyToClipboard">📋 Copy to clipboard</button>
            </article>
        </main>
        <section class="selection">
            <h2>Crazy Selector</h2>
            <input type="range" min="0" max="50" step="1" v-model="rangeValue" />
        </section>
        <Instructions />
    </div>
</template>

<script lang="ts" setup>
import Instructions from "./components/Instructions.vue";
import { ref, watch } from "vue";

const inputText = ref("");
const output = ref("");
const rangeValue = ref(1);

// prettier-ignore
const diacriticsUnicdeNumbers = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9", "A", "B", "C", "D", "E", "F"];
const diacriticsUnicodeBase = ["u030", "u031", "u032", "u033", "u034", "u035", "u036"];

function generateDiacritics(char: string, number: number) {
    // for each character in the input text generate x random diacritics
    let diacriticChar = "";
    for (let i = 0; i < number; i++) {
        const diacritic =
            diacriticsUnicodeBase[Math.floor(Math.random() * diacriticsUnicodeBase.length)] +
            diacriticsUnicdeNumbers[Math.floor(Math.random() * diacriticsUnicdeNumbers.length)];
        diacriticChar += `\\` + diacritic;
    }
    return char + diacriticChar;
}

function showOutput(inp: string, crazy: number) {
    let finalString = "";
    for (let i = 0; i < inputText.value.length; i++) {
        const char = inp[i];

        finalString += generateDiacritics(char, crazy);
    }
    // make it render the html
    output.value = finalString.replace(/\\u([a-f0-9]{4})/gi, (_n, hex) => {
        return String.fromCharCode(parseInt(hex, 16));
    });
}

function copyToClipboard() {
    navigator.clipboard.writeText(output.value);
}

watch(inputText, () => {
    showOutput(inputText.value, rangeValue.value);
});

watch(rangeValue, () => {
    showOutput(inputText.value, rangeValue.value);
});
</script>

<style>
:root {
    --bg-color: hsl(0, 0%, 10%);
    color-scheme: light dark;
}

* {
    margin: 0;
    padding: 0;
}

body {
    background-color: var(--bg-color);
    color: white;
    font-family: "Arial", sans-serif;
}

#app {
    display: flex;
    flex-direction: column;
    margin-inline: auto;
    max-width: 80rem;
    padding-inline: 1rem;
}

@media screen and (min-width: 400px) {
    #app {
        padding-inline: 0.5rem;
    }
}

h1 {
    padding-block: 1rem;
}

main {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1rem;
}

main textarea,
main .container {
    flex: 1;
    min-width: min(250px, 100%);
    font-size: 1.5rem;
    /* height: 15rem; */
    height: auto;
}

main textarea {
    padding: 1em;
}

.container {
    display: flex;
    flex-flow: column;
    gap: 1rem;
}

section {
    padding-block: 1rem;
}

h2 {
    padding-block: 0.5rem;
}

textarea {
    resize: vertical;
}

.output {
    border: 1px solid black;
    min-width: 250px;
    min-height: 100px;
    padding: 1em;
    font-size: 1.5rem;
    height: 100%;
}

.selection {
    display: flex;
    flex-direction: column;
}
</style>
