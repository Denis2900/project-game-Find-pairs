<template>
    <button class="standartButton">
        <slot></slot>
    </button>
</template>
<script>
export default{
    name:'StandartButton'
}
</script>

<style scoped>
    .standartButton{
         padding: 1%;
         font-size: 1.5rem;
         border: 2px solid orangered;
         border-radius: 5px;
    }
</style>