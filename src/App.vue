<template>
    <div class="container">
      <ModalWindow  v-show="visibledModalWindow" :gameOverObject="gameOverObject" @newGame="newGame"/>
      <Header @size="createArrayOfCubs" v-show="visibledHeader"></Header>
      <PlayingField v-show="visibledPlayingField" :array="arrayOfCubs" :numberOfCards="numberOfCard" @gameOver="showModalWindow" :levelTime="levelTime"/>
    </div>
</template>
<script>
  import Header from "@/components/Header"
  import PlayingField from "@/components/PlayingField"
  import ModalWindow from "@/components/ModalWindow"
 export default{
   data(){
     return{
        arrayOfCubs:[],
        visibledHeader:true,
        visibledModalWindow:false,
        visibledPlayingField:false,
        numberOfCard:0,
        gameOverObject:{statusWin:true,time:'',openCards:0,numberOfHelp:0},
        levelTime:0
     }
   },
   components:{
     Header,
     PlayingField,
     ModalWindow,
   },
   methods:{
     createArrayOfCubs(objectLevel){
       let {size,time} = objectLevel
       this.numberOfCard = size
       this.levelTime = time
       this.visibledHeader = false
       this.visibledPlayingField = true
       const adress = ['fish.png','dog.png','cat.png','chick.png','dinosauor.png','bear.png']
       const array = []
       size = Number(size)
       for(let i = 0; i < size/8;i++){
         let row = []
         for(let j = 0; j < 8;j+=2){
           let indexOfAdress = this.randomIndex(0,6)
           row[j] = adress[indexOfAdress]
           row[j+1] = adress[indexOfAdress]
         }
         array.push(row)
       };
        this.arrayOfCubs = this.chaotingSorting(array,size)
        console.log(this.arrayOfCubs,'arrayOfCubs');
        return array
     },
     randomIndex(min,max){
       return Math.floor(Math.random()*(max-min))+min
     },
     chaotingSorting(array,size){
       for(let i = 0; i < array.length;i++){
         for(let j = 0; j < 6;j++){
           let rowIndex = this.randomIndex(0,size/8)
           let columnIndex = this.randomIndex(0,8)
           let element
           element = array[i][j]
           array[i][j] = array[rowIndex][columnIndex]
           array[rowIndex][columnIndex] = element
         }
       }
       return array
     },
     showModalWindow(gameOverObject){
       this.visibledModalWindow = true
       const arrayOfKeys = Object.keys(this.gameOverObject)
       arrayOfKeys.forEach((key)=>{
         this.gameOverObject[key] = gameOverObject[key]
       })
     },
     newGame(newGameObject){
        this.visibledModalWindow = false
        let size = this.arrayOfCubs.length*8
           let objectTime = {
            '32':'5',
            '48':'6',
            '64':'7'
        }
        this.arrayOfCubs = []
        if(newGameObject.restart){
          this.arrayOfCubs = [...this.createArrayOfCubs({size:size,time:objectTime[size]})]
        }
        else{
          this.visibledHeader = true
          this.visibledPlayingField = false
        }
     }
   }
 }
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  @font-face {
    font-family: 'open-sans';
    src: url(./style/fonts/OpenSans/OpenSans-SemiBold.ttf);
  }
  .container{
    min-height: 100vh;
    background: #000;
    font-family: 'open-sans';
    color: #fff;
  }
</style>
