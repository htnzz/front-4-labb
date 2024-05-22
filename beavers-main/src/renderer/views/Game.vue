<template>
    <div class="game">
        <Board />
    </div>

    <!-- Передать сюда переменную GameStatus, чтобы воспроизводить видео -->
</template>

<script lang="ts">
import { defineComponent } from "vue";
import GameButton from "../components/GameButton.vue";
import Leaderboard from "../views/Leaderboard.vue"; // импортируем компонент Leaderboard
import http from "../http_common";
import Board from "../components/Board.vue";


export default defineComponent({
    components: {
        GameButton,
        Leaderboard, // добавляем компонент Leaderboard в компоненты
        Board,
    },

    data() {
        return {
            currScore: 0,
            counter: 20,
            score: 0,
            isLeftBeaver: true,
            isRightBeaver: false,
            isRightLog: true,
            isLeftLog: false,
            caught: false,
            user: null,
            userscore: 0,
        };
    },
    methods: {
        increment() {
            if (this.isRightBeaver && this.isRightLog && !this.caught) {
                this.caught = true;
                this.currScore++;
            } else {
                if (this.isLeftBeaver && this.isLeftLog && !this.caught) {
                    this.caught = true;
                    this.currScore++;
                }
            }
        },
        countDown() {
            if (this.counter) {
                return setTimeout(() => {
                    --this.counter;
                    this.countDown();
                }, 1000);
            }

            this.score = this.currScore;
        },
        toLeftBeaver() {
            this.isRightBeaver = false;
            this.isLeftBeaver = true;
        },
        toRightBeaver() {
            this.isLeftBeaver = false;
            this.isRightBeaver = true;
        },
        toLeftBounty() {
            setTimeout(() => {
                this.isLeftLog = true;
                this.isRightLog = false;
                this.caught = false;
            }, 1000);
        },
        toRightBounty() {
            setTimeout(() => {
                this.isLeftLog = false;
                this.isRightLog = true;
                this.caught = false;
            }, 1000);
        },
        bountyLoop() {
            if (this.isRightLog) {
                this.toLeftBounty();
            } else {
                this.toRightBounty();
            }
        },
        randomNum() {
            var random = Math.random();

            if (random < 0.34) return 1;

            return random;
        },
        handleSubmit() {
            if (this.score > this.userscore) {
                const response = http.put('/user/update/', {
                    score: this.score,
                });
            }
        },
    },
    async mounted() {
        this.countDown();
        await http
            .get('/user/')
            .then((response) => {
                this.user = response.data;
                this.userscore = response.data.score;
                console.log(response);
            })
            .catch((e) => {
                console.log(e);
            });
    },
    beforeUpdate() {
        this.increment();
    },
    updated() {
        this.bountyLoop();
    },
});
</script>

<style lang="css" scoped>
.objects {
    display: flex;
    justify-content: space-evenly;
    width: 70%;
}

.game-over {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #2f1e1e;
    background-color: #b85fac;
    border-radius: 5px;
    font-size: 26px;
    padding: 40px;
    font-weight: 700;
}

.bounty-rune {
    width: 20%;
}

.btn {
    display: flex;
    justify-content: space-evenly;
    width: 50%;
    margin-top: 30px;
}

.game {
    text-align: center;
    margin-top: 60px;

    user-select: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
}

a {
    text-decoration: none;
}

.beaver-img {
    width: 50%;
}

.bg {
    display: flex;
    flex-direction: column;
    height: 100vh;
}

.logs-img {
    width: 96%;
    z-index: 1;
    position: absolute;
    left: 0;
    bottom: 0;
    transform: scale(1.08, 1);
}

.header {
    background-color: #060223;
    font-size: 30px;
    text-align: center;
    padding: 20px;
    color: #7f9e9f;
    font-weight: 700;
    z-index: 2;
}

.timer {
    font-size: 26px;
    padding: 30px;
    font-weight: 700;
}

.score-info {
    font-size: 22px;
    font-size: 26px;
    margin-bottom: 60px;
    font-weight: 700;
}
</style>
