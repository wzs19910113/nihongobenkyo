<template>
    <div class="home">
        <div class="home-main" v-if="state==1">
            <h1 class="title">五十音训练</h1>
            <div class="block setup">
                <a class="btn" @click="onClickStart(1)">开始无限随机</a>
                <a class="btn" @click="onClickStart(2)">开始序列随机</a>
            </div>
        </div>
        <div class="home-main" v-if="state==2">
            <a class="btn btn-back" @click="onClickBack">返回</a>
            <div class="progress" v-if="mode==2">{{seqIndex+1}}/{{charSeq.length}}</div>
            <div class="block quiz" v-if="quizType!=0">
                <div class="info hiragana">
                    <input v-model="inp1" v-if="quizType==1||quizType==3" placeholder="平假名" />
                    <p :class="(quizType!=2&&showAnswer)?(inp1==char[0]?'correct':'alert'):''" v-if="quizType==2||showAnswer">{{char[0]}}</p>
                </div>
                <div class="info katakana">
                    <input v-model="inp2" v-if="quizType==1||quizType==2" placeholder="片假名" />
                    <p :class="(quizType!=3&&showAnswer)?(inp2==char[1]?'correct':'alert'):''" v-if="quizType==3||showAnswer">{{char[1]}}</p>
                </div>
                <div class="info spell">
                    <input v-model="inp3" v-if="quizType==2||quizType==3" placeholder="拼音" />
                    <p :class="(quizType!=1&&showAnswer)?(inp3==char[2]?'correct':'alert'):''" v-if="quizType==1||showAnswer">{{char[2]}}</p>
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
            state: 0, // 阶段[1:预设|2:进行中]
            mode: 0, // 模式[1:无限随机|2:序列随机]

            charSeq: [], // 序列
            seqIndex: -1, // 序列随机下标

            quizType: 0, // 题目类型[0:不显示|1:读音添字|2:读平添音片|3:读片添音平]
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
            this.state = 1;
            this.mode = 0; // 模式[1:无限随机|2:序列随机]

            this.charSeq = []; // 序列
            this.seqIndex = -1; // 序列随机下标

            this.quizType = 0; // 题目类型[0:不显示|1:读音添字|2:读平添音片|3:读片添音平]
            this.char = [];
            this.showAnswer = false;
            this.inp1 = '';
            this.inp2 = '';
            this.inp3 = '';
        },
        onClickStart(flag){
            this.mode = flag;
            this.state = 2;
            if(this.mode==2){ // 序列随机，排好一个序列
                this.charSeq = shuffle(CONFIG.chars);
            }
        },
        onClickCheck(){
            this.showAnswer = true;
        },
        onClickNext(){
            this.showAnswer = false;
            if(this.mode==1){ // 无限随机
                this.char = CONFIG.chars[r(0,CONFIG.chars.length-1)];
            }
            else if(this.mode==2){ // 序列随机
                this.seqIndex += 1;
                this.char = this.charSeq[this.seqIndex];
                if(this.seqIndex>this.charSeq.length-1){
                    this.init();
                }
            }
            this.quizType = r(1,3);
            this.inp1 = '';
            this.inp2 = '';
            this.inp3 = '';
        },
        onClickBack(){
            this.init();
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
    .title{
        height: 120px;
        line-height: 120px;
        text-align: center;
    }
    .setup{
        position: absolute;
        top: 200px;
        left: 0;
        right: 0;
        width: 100%;
        height: 100px;
    }
    .progress{
        position: absolute;
        left: 0;
        right: 0;
        margin: 0 auto;
        top: 10px;
        width: 100px;
        height: 25px;
        line-height: 25px;
        font-size: 18px;
        font-weight: bold;
        text-align: center;
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
    .btn-back{
        position: absolute;
        left: 0;
        top: 0;
        width: 50px;
        height: 30px;
        line-height: 30px;
        font-size: 16px;
        z-index: 500;
    }
</style>
