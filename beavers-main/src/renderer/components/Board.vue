<template>
    <div>Уровень {{ lvl }}</div>
    <div class="board" :style="{ width: 68 * size + 'px', height: 68 * size * 'px'}">
        <BoardItem
            v-for="(cell, index) in cells"
            :key="cell + ' ' + index"
            :icon="cell"
            @mousedown="mousedown(index)"
            @mouseup="mouseup(index)"
            @mousemove="go(index)"
            :selected="checkItem(index)"
            :closed="isPathClosed(index)"
        />

    </div>

    <div @click="reload()" class="reload">Сбросить</div>
</template>

<script>
import BoardItem from "../components/BoardItem.vue";
import { ref } from 'vue';

export default {
    name: 'Board',

    components: {
        BoardItem
    },

    setup() {
        let cells = ref([0, 2, 0, 4, 1, 0, 0, 3, 0, 0, 0, 0, 7, 0, 0, 9]);
        const path = ref([]);
        const size = ref(4);
        const walkedPath = ref([]);
        const maxLvl = 1;
        let gameOver = ref(false);
        let lvl = ref(1);

        const mousedown = (index) => {
            path.value = [];

            if (cells.value[index] && !isPathClosed(index)) {
                path.value.push(index);
            } 
        }

        const mouseup = (index) => {
            if (index !== path.value[0] && Math.abs(cells.value[index] - cells.value[path.value[0]]) === 2 && cells.value[index] !== 0) {
                walkedPath.value = walkedPath.value.concat(path.value);
            }

            path.value = [];

            checkLvl();
        }

        const checkLvl = () => {
            let completed = true;

            cells.value.forEach((cell, index) => {
                if (cell && !isPathClosed(index)) {
                    completed = false;
                }
            })

            if (completed) {
                alert('ya lox');
                goToNextLvl();
            }
        }

        const goToNextLvl = () => {
                lvl.value += 1;

                if (lvl.value > maxLvl) {
                    // lvl.value = 1;
                    gameOver.value = true;
                }
                start(lvl.value);
            }
        
        const go = (index) => {
            if (path.value.length) {
                const lastIndex = path.value[path.value.length - 1];

                if ((Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value) && !isPathClosed(index) && !checkItem(index)) {
                    path.value.push(index);
                }

                if (isPathClosed(index)){
                    path.value = [];
                }
            }

        }

        const checkItem = (index) => {
            return path.value.findIndex(p => p === index) > -1;
        }

        const isPathClosed = (index) => {
            return walkedPath.value.findIndex(p => p === index) > -1;
        }

        const start = (lvl) => {
            if (lvl === 1){
                gameOver.value = false;
                cells.value = [2, 0, 0, 4, 1, 0, 0, 3, 0, 0, 0, 0, 7, 0, 0, 9]
                size.value = 4;
            }
            if (lvl === 2) {
                cells.value = [0, 2, 1, 7, 0, 0, 0, 0, 0, 0, 0, 0, 4, 3, 9, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                size.value = 5;
                
            }
            path.value = [];
            walkedPath.value = [];


        }

        start(lvl.value);

        const reload = () => {
            start(lvl.value);
        }

        return {
            cells,
            mousedown,
            mouseup,
            go,
            checkItem,
            isPathClosed,
            lvl,
            reload,
            size, 
            gameOver,
        }
    }
}

</script>

<style>
.board {
    display: flex;
    flex-wrap: wrap;
    margin: 20px auto;
}   

</style>
