<template>
    <div class="modal-shape">
        <div class="modal-window modal-window__animation">
            <div class="modal-window__title" v-if="gameOverObject.statusWin">
                Поздравляем, вы выиграли
            </div>
            <div class="modal-window__title" v-else>
                К сожалению, вы проиграли
            </div>
            <div class="modal-window__time">
                Затраченное время - {{gameOverObject.time}}
            </div>
            <div class="modal-window__openCards">
                Открыты карточки - {{gameOverObject.openCards}}
            </div>
            <div class="modal-window__help">
                Использованные подсказки - {{gameOverObject.numberOfHelp}}
            </div>
            <div class="modal-window__button-block">
                <StandartButton style="margin:2% 0" @click="$emit('newGame',{restart:true})">Начать заного</StandartButton>
                <StandartButton style="margin:2% 0" @click="$emit('newGame',{restart:false})">Главное меню</StandartButton>
            </div>
        </div>
    </div>
</template>
<script>
export default{
    props:{
        gameOverObject:{
            type:Object,
            required:true
        }
    }
}
</script>

<style scoped>
.modal-shape{
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    z-index: 100;
    left: 0;
    background: transparent;
}
.modal-window{
    position: absolute;
    width: 50%;
    background: #fff;
    font-size: 1.3rem;
    padding: 2%;
    color: #000;
    border: 2px solid orangered;
    border-radius: 20px;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
.modal-window__title{
    text-align: center;
    font-size: 2rem;
}
.modal-window__time,.modal-window__title,.modal-window__help{
    margin: 5% 0;
}
.modal-window__animation{
    animation: animation-modal-window 1s ease;
}
.modal-window__button-block{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}
@media(max-width:700px){
 .modal-window{
     width: 90%;
 }
}
@media(max-width:400px){
   .modal-window__button-block{
       justify-content: center;
   }
    .modal-window{
        font-size: 1.1rem;
    }
}
@keyframes animation-modal-window {
    0%{
        top: -50%;
    }
    100%{
        top:50%
    }
}
</style>