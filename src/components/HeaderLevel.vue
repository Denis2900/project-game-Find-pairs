<template>
    <div class="level">
        <div class="level__img">
            <img :src="require(`../style/icons/${content.imgLink}`)" alt=""/>
        </div>
        <div class="level__text-block">
            <div class="level__title">
            {{content.title}}
            </div>
            <div class="level__size">
                Количество карточек-{{content.size}}
            </div>
            <div class="level__time">
                время-{{content.time}} минут
            </div>
            <div class="level__button" @click="$emit('size',{size:content.size,time:content.time})">
                <StandartButton>Играть</StandartButton>
            </div>
        </div>
    </div>
</template>
<script>
export default{
   props:{
     content:{
        type:Object,
        require:true
     },
     imgLink:{
         type:String,
         require:true
     }
   },
   methods:{
       foo(){
          this.$emit('size',size)
       }
   }
}
</script>

<style scoped>
    .level{
        max-width:300px;
        margin: 2%;
        background: #fff;
        border: 1px solid orangered;
        font-size: 1.3rem;
        color: #000;
    }
    .level__text-block{
        padding: 2%;
    }
    .level__img{
        background: black;
    }
    .level__title,.level__size,.level__time,.level__button{
        margin: 5% 0;
    }
    img{
        width: 100%;
    }
</style>