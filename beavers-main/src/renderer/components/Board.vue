<template>
    <CountdownTimer />
    <div>Счёт: {{ userscore }}</div>
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
    <div>
        <audio ref="audioPlayer" autoplay>
            <source src="../assets/soundtrack.mp3" type="audio/mpeg">
        </audio>
    </div>

    <div @click="reload()" class="reload">Сбросить</div>
</template>

<script>
import CountdownTimer from "../components/CountdownTimer.vue"
import BoardItem from "../components/BoardItem.vue";
import { ref } from 'vue';

export default {
    name: 'Board',

    components: {
        BoardItem,
        CountdownTimer,
    },

    mounted() {
            this.$refs.audioPlayer.play();
            this.$refs.audioPlayer.volume = 0.1;
        },

    setup() {
        const cellsLvl1 = ref([
            ref([1, 0, 4, 7, 0, 2, 0, 0, 0, 3, 0, 0, 9, 0, 0, 0]),
            ref([2, 1, 0, 0, 0, 7, 0, 0, 0, 0, 9, 3, 0, 0, 0, 4]),
            ref([7, 0, 0, 1, 3, 0, 0, 0, 2, 0, 9, 0, 4, 0, 0, 0]),
        ]);
        const cellsLvl2 = ref([
            ref([0, 0, 0, 1, 2, 0, 0, 0, 0, 4, 0, 7, 0, 0, 10, 0, 8, 0, 9, 0, 3, 0, 0, 0, 0]),
            ref([1, 2, 7, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 9, 0, 0, 0, 8, 0, 0, 0, 0, 3, 10]),
            ref([8, 0, 0, 0, 0, 1, 10, 0, 0, 0, 0, 0, 0, 0, 0, 7, 0, 4, 8, 2, 0, 9, 0, 0, 0]),
        ]);
        const cellsLvl3 = ref([
            ref([1, 0, 2, 0, 0, 0, 7, 0, 8, 0, 0, 0, 0, 3, 0, 13, 0, 0, 0, 10, 0, 0, 0, 0, 0, 0, 0, 9, 0, 0, 0, 0, 0, 15, 0, 4]),
            ref([2, 0, 3, 0, 0, 0, 0, 1, 8, 0, 7, 0, 0, 0, 0, 13, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 9, 10, 0, 0, 0, 4, 0, 0, 0, 15]),
            ref([0, 1, 0, 0, 0, 0, 4, 0, 0, 0, 0, 3, 7, 0, 10, 0, 0, 2, 0, 0, 0, 0, 0, 15, 0, 9, 0, 0, 0, 0, 0, 0, 12, 13, 0, 0]),
        ]);

        let cells = ref([0, 2, 0, 4, 1, 0, 0, 3, 0, 0, 0, 0, 7, 0, 0, 9]);
        const path = ref([]);
        const size = ref(4);
        const walkedPath = ref([]);
        const maxLvl = 100; // Это убери да
        const volume = 0.3;
        let gameOver = ref(false); // Это тоже убери да
        let lvl = ref(1);
        let userscore = ref(0);
        let randomIndex = ref(0);

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
                goToNextLvl();
            }
        }

        const goToNextLvl = () => {
                lvl.value += 1;
                userscore.value += 1;
                if (lvl.value > 3) {
                    lvl.value = 1
                }

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
                randomIndex.value = Math.floor(Math.random() * 3);
                
                gameOver.value = false;
                cells.value = cellsLvl1.value[randomIndex.value].value;
                
                size.value = 4;
            }
            if (lvl === 2) {
                randomIndex.value = Math.floor(Math.random() * 3);
                cells.value = cellsLvl2.value[randomIndex.value].value;
                size.value = 5;
                
            }
            if (lvl === 3) {
                randomIndex.value = Math.floor(Math.random() * 3);
                cells.value = cellsLvl3.value[randomIndex.value].value;
                size.value = 6;
                
            }
            path.value = [];
            walkedPath.value = [];


        };

        start(lvl.value);

        const reload = () => {
            start(lvl.value);
        };
        
        return {
            cells,
            cellsLvl1,
            cellsLvl2,
            cellsLvl3,
            randomIndex,
            mousedown,
            mouseup,
            go,
            checkItem,
            isPathClosed,
            lvl,
            userscore,
            reload,
            size, 
            gameOver,
        };
    }
}

</script>

<style>
.board {
    display: flex;
    flex-wrap: wrap;
    margin: 20px auto;
}   
audio {
  display: none; /* Скрыть аудиоплеер */
}

</style>
