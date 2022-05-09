<template>
    <div class="home">
        <div class="home-main">
            <div class="block quiz" v-if="mode!=0">
                <div class="info hiragana">
                    <input v-model="inp1" v-if="mode==1||mode==3" placeholder="平假名" />
                    <p :class="(mode!=2&&showAnswer)?(inp1==char[0]?'correct':'alert'):''" v-if="mode==2||showAnswer">{{char[0]}}</p>
                </div>
                <div class="info katakana">
                    <input v-model="inp2" v-if="mode==1||mode==2" placeholder="片假名" />
                    <p :class="(mode!=3&&showAnswer)?(inp2==char[1]?'correct':'alert'):''" v-if="mode==3||showAnswer">{{char[1]}}</p>
                </div>
                <div class="info spell">
                    <input v-model="inp3" v-if="mode==2||mode==3" placeholder="拼音" />
                    <p :class="(mode!=1&&showAnswer)?(inp3==char[2]?'correct':'alert'):''" v-if="mode==1||showAnswer">{{char[2]}}</p>
                </div>
            </div>
            <div class="block ops">
                <a class="btn" @click="onClickCheck" v-if="char.length>0">検查</a>
                <a class="btn" @click="onClickNext" v-if="showAnswer||char.length==0">つぎ</a>
            </div>
        </div>
    </div>
</template>

<script>
import { query, r, rr, bulbsort, shuffle, getParentNode, getMatchList, removeFromList, arrContains, removeFromNumberList, } from '../tools/utils';
import { DEBUG, CONFIG, } from '../config/config';
export default {
    name: 'Home',
    data(){
        return {
            state: 0,

            mode: 0, // 模式[0:不显示|1:读音添字|2:读平添音片|3:读片添音平]
            char: [],
            showAnswer: false,
            inp1: '',
            inp2: '',
            inp3: '',
        };
    },
    destroyed(){

    },
    created(){
        this.init();
    },
    mounted(){
    },
    methods: {
        init(){

        },
        onClickCheck(){
            this.showAnswer = true;
        },
        onClickNext(){
            this.showAnswer = false;
            this.char = CONFIG.chars[r(0,CONFIG.chars.length-1)];
            this.mode = r(1,3);
            this.inp1 = '';
            this.inp2 = '';
            this.inp3 = '';
        },
    },
    components:{
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .home{
        position: relative;
        width: 100%;
        height: 100%;
    }
    .home-main{
        position: absolute;
        left: 0;
        right: 0;
        top: 10%;
        margin: 0 auto;
        width: 800px;
        height: 400px;
        background-color: #fff;
        color: #000;
    }
    .block{
        width: 100%;
    }
    .quiz{
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        width: 100%;
        height: 300px;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }
    .ops{
        position: absolute;
        top: 300px;
        left: 0;
        right: 0;
        width: 100%;
        height: 100px;
    }
    .info{
        position: relative;
        width: 30%;
        height: 300px;
    }
    .info input,
    .info p{
        position: absolute;
        margin: 0 auto;
        left: 0;
        right: 0;
        display: block;
        -webkit-appearance: none;
        width: 70%;
        height: 70px;
        line-height: 70px;
        font-size: 30px;
        font-weight: bold;
        color: BlueViolet;
        text-align: center;
    }
    .info p{
        bottom: 150px;
        border: 2px solid transparent;
    }
    .info input{
        background-color: PaleTurquoise;
        color: SlateBlue;
        border: 1px solid BlueViolet;
        bottom: 50px;
    }
    .info input::placeholder{
        color: LightCyan;
    }
    .correct{
        border: 2px solid LimeGreen !important;
    }
    .alert{
        border: 2px solid Crimson !important;
    }
    .btn{
        display: block;
        color: #fff;
        width: 200px;
        height: 40px;
        margin: 0 auto;
        line-height: 40px;
        text-align: center;
        font-weight: bold;
        font-size: 20px;
        white-space: nowrap;
        word-break: keep-all;
        margin-bottom: 4px;
        background-color: OrangeRed;
        cursor: pointer;
    }
    .btn:hover{
        opacity: .75;
    }
</style>
