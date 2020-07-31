<template>
    <div class="simon-block">
        <h1>Simon Says</h1>
        <div class="content">
            <div class="content__button">
                <button 
                    v-for="(item, index) in color" 
                    :key="index"
                    :disabled="!userPlayed"
                    @click="play(item.id)"
                    :class="{played: item.id === id}"
                    :style="{backgroundColor: item.color}"
                ></button>
            </div>
            <p>Choose level:</p>
            <div>
                <label><input :disabled="played" v-model="level" name="level" type="radio" :value="1500"> Easy</label>
                <label><input :disabled="played" v-model="level" name="level" type="radio" :value="1000"> Medium</label>
                <label><input :disabled="played" v-model="level" name="level" type="radio" :value="400"> Hard</label>
            </div>
            <button v-show="!played" @click="start()">Start</button>
            <button v-show="played" @click="stop()">Stop</button>
            <div v-show="gameOver">Sorry, you lose after {{count}} rounds!</div>
        </div>
        {{arr}}
    </div>
</template>

<script>
    export default {
        data() {
            return {
                color: [
                    {id: 1, color: '#FF5643'},
                    {id: 2, color: 'dodgerblue'},
                    {id: 3, color: '#FEEF33'},
                    {id: 4, color: '#BEDE15'}
                ],
                id: null,
                played: false,
                gameOver: false,
                userPlayed: false,
                count: 0,
                arr: [],
                userArr: [],
                userClickId: 0,
                clickId: 0,
                level: 1500 // Ничего не делает
            }
        },
        methods: {
            play(id) {
                if(this.played)
                {
                    if(this.userPlayed && id) {
                        this.id = id;
                        this.userArr.push(id);
                        let arr = this.arr[this.userClickId];
                        setTimeout(() => {
                            this.id = null;
                        }, this.level / 5);
                        console.log(arr != this.userArr[this.userClickId], this.arr, arr, this.userArr[this.userClickId], this.userClickId, this.userArr)
                        if(arr != this.userArr[this.userClickId]) {
                            alert('Game over!');
                            this.gameOver = true;
                            this.stop();
                        }
                        else {
                            this.userClickId++;
                            if(this.arr.length === this.userClickId) {
                                setTimeout(() => {
                                    this.userPlayed = false;
                                    console.log('Next scene')
                                    this.userClickId = 0;
                                    this.userArr = []
                                }, 500);
                            }
                        }
                    }
                    else if(!this.userPlayed) {
                        let length = this.arr.length;
                        if(this.clickId == length) {
                            let num = Math.floor(Math.random() * (5 - 1)) + 1
                            this.arr.push(num);
                            this.id = num;
                            this.userPlayed = true;
                            this.clickId = 0;
                        }   
                        else {
                            this.id = this.arr[this.clickId];
                            this.clickId++;
                        }
                        setTimeout(() => {
                            this.id = null;
                        }, this.level / 5);
                    }
                }
            },
            start() {
                this.played = true;
                this.gameOver = false;
                this.count = 0;
                this.userPlayed = false;
            },
            stop() {
                this.played = false; 
                this.userClickId = 0;
                this.count = 0;
                this.userArr = [],
                this.arr = [];
                this.clickId = 0;
                this.userPlayed = false;
            },
        },
        mounted () {
            // this.data()
            setInterval(this.play.bind(this) , this.level);
        },
    }
</script>

<style scoped>
    .simon-block {
        width: 100%;
        overflow: hidden;
        height: 100vh;
    }
    h1 {
        text-align: center;
    }
    .content__button {
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        width: 300px;
        border-radius: 50% 50%;
        overflow: hidden;
    }
    .content__button > button {
        width: 150px;
        height: 150px;
        opacity: 0.6;
        cursor: pointer;
        border: 1px solid #000;
        box-sizing: border-box;
        transition: border .3s;
    }
    .content__button > button:disabled {
        cursor: default;
    }
    .content__button > button:not(:disabled):hover {
        border: 3px solid #000;
    }
    .content__button > button.played {
        opacity: 1;
        border-right: none;
        border-bottom: none;
    }
    .content__button > button:nth-child(1) {
        border-radius: 150px 0 0 0;
        border-right: none;
        border-bottom: none;
    }
    .content__button > button:nth-child(2) {
        border-radius: 0 150px 0 0;
        border-left: none;
        border-bottom: none;
    }
    .content__button > button:nth-child(3) {
        border-radius: 0 0 0 150px;
        border-right: none;
        border-top: none;
    }
    .content__button > button:nth-child(4) {
        border-radius: 0 0 150px 0;
        border-left: none;
        border-top: none;
    }
</style>