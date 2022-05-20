<template>
    <div class="field">
        <div class="field__timer" v-show="stopTimer">
            Оставшиеся время {{this.timerMinutes}}:<span v-show="timerSeconds<10">0</span>{{this.timerSeconds}}
        </div>
        <div class="field__helps">
            <p> Подсказки <img src="@/style/icons/helps.png"/></p> <p>x <span class="field__number-Of-helps">{{numberOfHelps}}</span></p> <StandartButton style="font-size: 1.3rem;" @click="usingHelps">Применить</StandartButton>
        </div>
        <div class="row" v-for="element,ind in array" v-bind:key="ind">
           <div class="cube" v-for="elem,index in element" v-bind:key="index+ind" @click="rotationCube" :addres="`../style/icons/${elem}`">
                <div class="cube__side cube__front-side">
                     <img src="../style/icons/question.png" alt="question">
                </div>
                <div class="cube__side cube__back-side">
                     <img :src="require(`../style/icons/${elem}`)" :alt="`${elem}_asnwer`">
                </div>
                <div class="cube__side cube__right-side"></div>
                <div class="cube__side cube__left-side"></div>
            </div>
        </div>
    </div>
</template>
<script>
export default{
    data(){
        return{
            currentCube:null,
            waiting:false,
            openCards:0,
            timerMinutes:0,
            numberOfHelps:3,
            timerSeconds:0,
            stopTimer:false,
            buttonHelp:null
        }
    },
    props:{
        array:{
            type:Array,
            required:true
        },
        numberOfCards:{
            type:Number,
            required:true
        },
        levelTime:{
            type:Number,
            required:true
        }
    },
    methods:{
        rotationCube(event){
            if(!this.stopTimer){
                this.timerSeconds = 59
                this.timerMinutes = this.levelTime-1
                this.stopTimer = true
            }
            if(event.currentTarget.classList.contains('rotation-front') || this.waiting){
                return null
            }
            event.currentTarget.classList.remove('rotation-back')
            event.currentTarget.classList.add('rotation-front')
            this.playSound('buttonClick')
            if(this.currentCube){
                this.waiting = true
                if(this.currentCube.getAttribute('addres')!==event.currentTarget.getAttribute('addres')){
                    const cube = event.currentTarget
                    setTimeout(()=>{;
                        cube.classList.add('rotation-back')
                        cube.classList.remove('rotation-front')
                        if(this.currentCube){
                            this.currentCube.classList.add('rotation-back')
                            this.currentCube.classList.remove('rotation-front')   
                        }
                        this.currentCube = null
                        this.waiting = false
                    },1500) 
                }
                else{
                    this.openCards+=2
                     if(this.checkWin(event.currentTarget)){
                        return null
                    }
                    setTimeout(this.playSound('tryAnswer'),1000)
                    this.currentCube = null
                    this.waiting = false
                }
            }
            else{
                this.currentCube = event.currentTarget
            }
        },
        playSound(src){
            const audio = new Audio()
            audio.src = require(`../style/audio/${src}.mp3`)
            audio.autoplay = true
        },
        clearField(currentTarget){
            currentTarget.parentElement.parentElement.childNodes.forEach((child)=>{
                if(child.nodeName !== '#text' && child.classList.contains('row')){
                    child.childNodes.forEach((cub)=>{
                        if(cub.nodeName !== '#text'){
                          cub.classList.remove('rotation-front')
                          cub.classList.add('rotation-back')  
                        } 
                    })
                }
            })
             this.timerMinutes = 0
             this.timerSeconds = 0
             this.openCards = 0
             this.currentCube = null
             this.numberOfHelps = 3
             this.waiting = false
             this.buttonHelp.removeAttribute('disabled')
        },
        usingHelps(event){
             this.buttonHelp = event.target
             if(!this.currentCube){
                 return null
             }
             this.numberOfHelps--
             if(this.numberOfHelps===0){
                 event.target.setAttribute('disabled',true)
             }
             let cube = null
             let attribute = this.currentCube.getAttribute('addres') 
             this.currentCube.parentElement.parentElement.childNodes.forEach((child)=>{
                if(child.nodeName !== '#text' && child.classList.contains('row')){
                    child.childNodes.forEach((cub)=>{
                        if(cub.nodeName !== '#text' && cub.getAttribute('addres')===attribute && !cub.classList.contains('rotation-front')){
                          cub.classList.remove('rotation-back')
                          cub.classList.add('rotation-front')
                          cube = cub
                          attribute = ''
                        } 
                    })
                }
            })
            this.currentCube = null
            this.openCards+=2
            this.checkWin(cube)
            this.playSound('tryAnswer')
        },
        checkWin(cube){
            if(this.openCards === +this.numberOfCards){
                this.stopTimer= false
                setTimeout(this.playSound('gameWon'),1000)
                let minutes
                if(this.timerMinutes===0){
                    minutes = this.levelTime-1
                }
                else{
                    minutes = this.levelTime - this.timerMinutes - 1
                }
                let seconds = 0
                if(this.timerSeconds===0){
                    minutes++
                }
                else{
                    seconds = 60 - this.timerSeconds
                    if(seconds < 10){
                        seconds = '0'+seconds
                    }
                }
                this.$emit('gameOver',{statusWin:true,time:`${minutes}:${seconds}`,openCards:this.openCards,numberOfHelps:this.numberOfHelps})
                this.clearField(cube)
                return true
            }
            return null 
        }
    },
    watch:{
        timerSeconds:{
            handler(value){
                if(this.stopTimer){
                    if(value>0){
                        setTimeout(()=>{
                            this.timerSeconds--
                        },1000)
                    }
                    if(value===0){
                        if(this.timerMinutes === 0){
                            this.stopTimer = false
                            this.$emit('gameOver',{statusWin:false,time:`${this.levelTime}:00`,openCards:this.openCards,numberOfHelp:3})
                            this.playSound('gameLost')
                            return null
                        }
                        this.timerMinutes--
                        this.timerSeconds = 59
                    }
                }
            },
            immediate:true
        }
    }
}
</script>

<style scoped>
.field{
 position: relative;
 margin: 0 auto;
 padding: 5% 0;
 background: #000;
 width: 100%;
 perspective: 100%;
}
.cube{
    width: 100px;
    height: 100px;
    transform-style: preserve-3d;
}
.cube__side{
    border: 2px solid #fff;
    width: inherit;
    height: inherit;
    position: absolute;
}
.cube__front-side{
    transform: translateZ(15px);
    background: orangered;
}
.cube__back-side{
    transform: rotateY(180deg) translateZ(15px);
    background: orangered;
}
.cube__right-side{
    width: 30px;
    transform: rotateY(90deg) translateZ(85px);
   background: orange;
}
.cube__left-side{
    width: 30px;
    transform: rotateY(-90deg) translateZ(15px);
    background: orange;
}
.rotation-front{
    animation: cube-rotation-front 1s ease forwards;
}
.rotation-back{
    animation: cube-rotation-back 1s ease forwards;
}
.cube__front-side img{
    width: 100%;
}
.cube__back-side img{
    width: 100%;
}
.row{
    display: flex;
    justify-content: center;
    align-items: center;
}
.field__timer{
    margin: 0 0 5% 0;
    text-align: center;
    font-size: 2rem;
}
.field__helps{
    font-size: 1.5rem;
    max-width: 500px;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    margin: 5% 2%;
}
.field__number-Of-helps{
    font-size: 2rem;
}
@keyframes cube-rotation-front {
    0%{
        transform: rotateY(0deg);
    }
    100%{
        transform: rotateY(180deg);
    }
}
@keyframes cube-rotation-back {
    0%{
        transform: rotateY(180deg);
    }
    100%{
        transform: rotateY(360deg);
    }
}
@media (max-width:800px){
    .cube{
        width: 60px;
        height: 60px;
    }
    .cube__right-side{
        width: 30px;
        transform: rotateY(90deg) translateZ(45px);
        background: orange;
    }
}
@media (max-width:500px){
    .cube{
        width: 40px;
        height: 40px;
    }
    .cube__right-side{
        width: 30px;
        transform: rotateY(90deg) translateZ(25px);
        background: orange;
    }
}
@media (max-width:350px){
    .field__helps{
     display: block;
     margin: 5% auto;
     text-align: center;
     width: 100%;
    }
    .cube{
        width: 30px;
        height: 30px;
        transform-style: preserve-3d;
    }
    .cube__front-side{
        transform: translateZ(8px);
        background: orangered;
    }
    .cube__back-side{
        transform: rotateY(180deg) translateZ(8px);
        background: orangered;
    }
    .cube__right-side{
        width: 16px;
        transform: rotateY(90deg) translateZ(22px);
       background: orange;
    }
    .cube__left-side{
        width: 16px;
        transform: rotateY(-90deg) translateZ(8px);
        background: orange;
    }
}
</style>