<template>
    <div class="header">
        <div class="header__title">
            Игра-найди пары
        </div>
        <div class="header__sub-title">
            Выберите уровень игры 
        </div>
        <div class="header__level">
            <HeaderLevel :content="contentEaseLevel" @size="$emit('size',{size:contentEaseLevel.size,time:contentEaseLevel.time})"/>
             <HeaderLevel :content="contentMiddleLevel" @size="$emit('size',{size:contentMiddleLevel.size,time:contentMiddleLevel.time})"/>
            <HeaderLevel :content="contentDifficultLevel" @size="$emit('size',{size:contentDifficultLevel.size,time:contentDifficultLevel.time})"/>
        </div>
    </div>
</template>
<script>
import HeaderLevel from "@/components/HeaderLevel.vue"
export default{
    data(){
        return{
            contentEaseLevel:{
                title:'Простой уровень',
                size:'32',
                time:'5',
                imgLink:"EaseLevel.png"
            },
            contentMiddleLevel:{
                title:'Средний уровень',
                size:'48',
                time:'6',
                imgLink:'MiddleLevel.png'
            },
            contentDifficultLevel:{
                title:'Сложный уровень',
                size:'64',
                time:'7',
                imgLink:'DifficultLevel.png'
            }
        }
    },
    components:{
        HeaderLevel,
    }
}
</script>

<style scoped>
    .header{
        padding: 2% 0;
        font-size: 2rem;
    }
    .header__title{
        font-size: 2.5rem;
        text-align: center;
    }
    .header__sub-title{
        text-align: center;
        margin: 2% 0;
    }
    .header__level{
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
    }
</style>