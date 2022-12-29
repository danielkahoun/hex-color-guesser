<script>
export default {
    data() {
        return {
            settings: {
                difficulty: 5,
            },
            game: {
                guessingColor: null,
                victory: false,
                colors: [],
            }
        }
    },
    methods: {
        prepareGame() {
            this.game.victory = false;
            this.game.message = '';
            this.game.colors = this.generateColors();
            this.game.guessingColor = this.getRandomColor();
        },
        generateHexColor() {
            let hexColor = '#';
            for (let i = 0; i < 6; i++) {
                let random = Math.floor(Math.random() * 16);
                hexColor += random.toString(16);
            }
            return hexColor;
        },
        generateColors() {
            let colors = [];
            for (let i = 0; i < this.settings.difficulty; i++) {
                colors.push({ hex: this.generateHexColor(), hide: false });
            }
            return colors;
        },
        getRandomColor() {
            let random = Math.floor(Math.random() * this.game.colors.length);
            return this.game.colors[random].hex;
        },
        guessColor(c) {
            if (c.hex === this.game.guessingColor) {
                this.game.colors.forEach(c => {
                    c.hide = c.hex != this.game.guessingColor;
                });
                this.game.victory = true;
            } else {
                this.game.message = 'Try again. That color was <span style="color:'+c.hex+'">' + c.hex + '</span>';
                c.hide = true;
            }
        }
    },
    created() {
        this.$watch('settings.difficulty', () => {
            this.prepareGame();
        })
    },
    mounted() {
        this.prepareGame();
    }
}
</script>

<template>
    <div class="container">
        <h1>{{game.guessingColor}}</h1>
        <div class="colors">
            <div v-for="color in game.colors" :key="color.hex" :style="{ backgroundColor: (color.hide) ? '#FFFFFF' : color.hex }" @click="guessColor(color)" class="color"><span v-if="color.hex == game.guessingColor && game.victory == true">✓</span></div>
        </div>
        <h2>Guess the color</h2>

        <div v-if="game.victory">
            <h2>Correct!</h2>
            <button @click="prepareGame()">Start a new game</button>
        </div>
        <div v-else>
            <h2 v-html="game.message"></h2>
        </div>

        <p>Set difficulty</p>
        <div class="difficulty">
            <button type="button" @click="settings.difficulty = 2">2</button>
            <button type="button" @click="settings.difficulty = 3">3</button>
            <button type="button" @click="settings.difficulty = 4">4</button>
            <button type="button" @click="settings.difficulty = 5">5</button>
            <button type="button" @click="settings.difficulty = 8">8</button>
            <button type="button" @click="settings.difficulty = 10">😎</button>
            <button type="button" @click="settings.difficulty = 40">💀</button> <!-- tak akorát pro pana učitele -->
            <input type="number" v-model="settings.difficulty" /> <!-- možnost vlastní obtížnosti -->
        </div>
    </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Exo+2:wght@100&display=swap');

.container {
    margin: 0;
    padding: 0;
    font-family: 'Exo 2', sans-serif;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

h1 {
    font-size: 60px;
}

h2 {
    font-size: 20px;
    color: #333;
}

h1, h2, p {
    text-transform: uppercase;
    text-align: center;
}

.colors {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
}

.color {
    width: 60px;
    height: 60px;
    margin: 10px;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    font-size: 30px;
    transition: 0.2s;
}

.difficulty {
    display: flex;
    justify-content: center;
    align-items: center;
}

.difficulty button {
    margin: 10px;
    padding: 10px;
    width: 50px;
    height: 50px;
    border: none;
    border-radius: 50%;
    background: white;
    cursor: pointer;
}

.difficulty button:hover {
    background: rgb(195, 195, 195);
}

.difficulty input {
    margin: 10px;
    padding: 10px;
    width: 50px;
    height: 50px;
    outline: none;
    border: none;
    border-radius: 50%;
    background: rgb(222, 222, 222);
    text-align: center;
}
</style>