<template>
    <section>
        <h2>Instructions</h2>
        <p>
            Write some text in the input box and it will generate random diacritics for each
            character in the text.
        </p>
        <p>
            The diacritics are generated randomly and can be any of the following:
            <code>u030</code>, <code>u031</code>, <code>u032</code>, <code>u033</code>,
            <code>u034</code>, <code>u035</code>, <code>u036</code> followed by a number from 0 to 9
            or a letter from A to F.
        </p>
        <p>The output is in the form of a zalgotext, which is a text with random diacritics.</p>
    </section>
</template>
<script setup lang="ts"></script>
<style scoped>
section p {
    line-height: 1.5rem;
}
</style>
