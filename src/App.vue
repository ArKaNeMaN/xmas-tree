<script setup>

import {computed, ref, watch} from "vue";

const treeWidth = ref(13);
const treeHeight = ref(7);
const treeLogLength = ref(2);
const treeLogWidth = ref(3);
const lightsColors = [
    'red',
    'blue',
    'white',
    'green',
];

function getRandomInt(max) {
    return Math.floor(Math.random() * max);
}

function getRandomColor() {
    return lightsColors[getRandomInt(lightsColors.length)];
}

function buildTreeRows(w, h) {
    let rows = [];

    for (let i = 1; i <= h; ++i) {
        let rowW = Math.floor(w / h * i);

        let row = [];
        for (let j = 0; j < rowW; ++j) {
            row.push({
                color: getRandomColor(),
                char: '*',
            });
        }

        rows.push(row);
    }

    return rows;
}

const refreshHandle = ref(1);
const treeRows = computed(() => refreshHandle.value ? buildTreeRows(treeWidth.value, treeHeight.value) : []);

const refreshInterval = ref(300);
let lightsIntervalHandler = null;
watch(refreshInterval, function () {
    if (lightsIntervalHandler !== null) {
        clearInterval(lightsIntervalHandler);
    }

    lightsIntervalHandler = setInterval(() => refreshHandle.value = Math.random(), refreshInterval.value);
}, {
    immediate: true,
});

const showSettingsBlock = ref(false);

const leafSize = ref(45)

</script>

<template>
    <div class="tree-container" @click="showSettingsBlock = !showSettingsBlock">
        <div class="tree" :style="{'font-size': `${leafSize}px`}">
            <p v-for="row in treeRows" class="tree-row">
            <span v-for="leaf in row" class="tree-leaf" :style="{color: leaf.color}">
                {{ leaf.char }}
            </span>
            </p>
            <p v-for="bh of treeLogLength" class="tree-row tree-base-row">
                <span v-for="bw of treeLogWidth">|</span>
            </p>
        </div>
    </div>
    <div class="settings-container">
        <div v-if="showSettingsBlock" class="settings">
            <input
                placeholder="Интервал гирлянды, мс"
                type="number"
                v-model.number="refreshInterval"
            >
            <input
                placeholder="Ширина"
                type="number"
                v-model.number="treeWidth"
            >
            <input
                placeholder="Высота"
                type="number"
                v-model.number="treeHeight"
            >
            <input
                placeholder="Ширина ствола"
                type="number"
                v-model.number="treeLogWidth"
            >
            <input
                placeholder="Высота ствола"
                type="number"
                v-model.number="treeLogLength"
            >
            <input
                placeholder="Размер, px"
                type="number"
                v-model.number="leafSize"
            >
        </div>
    </div>
</template>

<style scoped>
.tree-container {
    width: 100vw;
    height: 95vh;
    margin: 0;
    padding: 16px;
    background-color: black;
    overflow: auto;
}

.tree {
    text-align: center;
    font-family: monospace;
}

.settings-container {
    height: 5vh;
    background-color: black;
    overflow-x: auto;
}

.settings {
    display: flex;
    align-items: center;
    padding-inline: 2em;
    justify-content: space-around;
}

.settings > * {
    margin-inline: 2px;
}

.tree-row {
    margin: 0;
    line-height: .9em;
}

.tree-base-row {
    color: sienna;
}
</style>
