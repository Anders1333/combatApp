<template>
    <Page>
        <ActionBar alignText="center" title="Combat Tracker">
            <Label fontSize="26" color="black" height="80px" width="*" text="Combat Tracker"/>
        </ActionBar>
        <StackLayout class="main-div" >
        <GridLayout class="center-grid"  columns="auto,auto,auto,auto,auto,auto,auto" rows="auto,auto,auto,auto,auto,auto,auto">
            <Button class="invisible-top" text="" col="0" row="0" @tap="increase(1)"/>
            <Button class="invisible-top" text="" col="1" row="0" @tap="increase(2)"/>
            <Button class="invisible-top" text="" col="2" row="0" @tap="increase(3)"/>
            <Button class="invisible-bot" text="" col="0" row="0" @tap="decrease(1)"/>
            <Button class="invisible-bot" text="" col="1" row="0" @tap="decrease(2)"/>
            <Button class="invisible-bot" text="" col="2" row="0" @tap="decrease(3)"/>
        

            <Label  class="chakra" :text="chakra" col="0" row="0"/>
            <Image class="back-image-1" src="~/assets/images/chakra-done.png" col="0" row="0"/>
            <Label  class="hitpoints" :text="hitpoints" col="1" row="0"/>
            <Image class="back-image-2" src="~/assets/images/hearth_done.png" col="1" row="0"/>
            <Label  class="stamina" :text="stamina" col="2" row="0"/>
            <Image class="back-image-3" src="~/assets/images/running_done.png" col="2" row="0"/>        
        </GridLayout>
        <GridLayout class="center-grid" columns="auto,auto,auto,auto,auto,auto,auto,auto" rows="auto,auto,auto,auto,auto,auto,auto,auto">

        
        <Button borderColor="#9d9d9d" borderRadius="50%" backgroundColor="white" borderWidth="2px"  height="80px" width="200px" text="Use" @tap="useChakra" col="0" row="0" />


        <Button class="invisible-big" text="" col="0" row="1" @tap="rest"/>
        <Image class="back-image-5" src="~/assets/images/rest-done.png" col= "0" row="1" />
        <Button class="invisible-big" text="" col="1" row="1" @tap="charge"/>
        <Image class="back-image-4" src="~/assets/images/charge-done.png" col="1" row="1"/>
        <Button class="invisible-big" text="" col="2" row="1" @tap="dodge"/>
        <Image class="back-image-7" src="~/assets/images/dodge-done.png" col="2" row="1"/>


        <Button class="invisible-big" text="" col="2" row="2" @tap="kunaiBlock"/>
        <Image class="back-image-6" :src="kunaiPicture" col="2" row="2"/>
        <Button class="invisible-big" text="" col="0" row="2" @tap="useShuriken"/>
        <Image class="back-image-7" :src="shurikenPicture" col="0" row="2"/>


       
        <Image class="back-image-8" src="~/assets/images/kunai-inventory-done.png" col="2" row="3"/>
        <Label class="small-text"   :text="kunai" col="2" row="4"/>
         <Button text="" class="invisible-top" col="2" row="3" @tap="addKunai"/>
        <Button text="" class="invisible-bot" col="2" row="3" @tap="deductKunai"/>
        
      
        <Image class="back-image-8" src="~/assets/images/shuriken-inventory-done.png" col="0" row="3"/>
        <Label class="small-text"  :text="shurikens" col="0" row="4"/>
        <Button text="" class="invisible-top" col="0" row="3" @tap="addShuriken"/>
        <Button  text="" class="invisible-bot" col="0" row="3" @tap="deductShuriken"/>

        <Label paddingTop="65px" text="Reset" fontSize="22" class="back-image-8" col="1" row="3" @tap="reset"/>
       



    
        </GridLayout>
       
        </StackLayout>
    </Page>
</template>

<script >
import * as dialogs from "tns-core-modules/ui/dialogs";
  export default {
    data() {
      return {
        msg: 'Hello!',
        chakra: '0',
        hitpoints: '20',
        stamina: '100',
        shurikens: '0',
        kunai: '0',
        kunaiEquipped: false,
        kunaiPicture: '~/assets/images/none-done.png',
        kunaiDurability: '0',
        shurikenPicture: '~/assets/images/none-done.png'
      }
     
    },
     methods: {

    increase(stat){
    switch(stat){
        case 1: this.chakra ++; return;
        case 2: this.hitpoints ++; return;
        case 3: this.stamina ++; return; 
    }
    
    },

    decrease(stat){
        switch(stat){
        case 1: this.chakra --; return;
        case 2: this.hitpoints --; return;
        case 3: this.stamina --; return; 
    }
    },

    charge(){
        if(this.stamina==0){
            dialogs.alert("You do not have enough stamina!")
        }
          this.chakra ++;
          this.chakra ++;
          this.stamina --;
    },


   

    reset(){
        this.chakra = 0;
        this.stamina = 100;
        this.hitpoints = 20;
        this.kunaiDurability = 0; 
        this.kunai = 0;
        this.shurikens = 0;
        this.kunaiPicture = '~/assets/images/none-done.png';
        this.shurikenPicture = '~assets/images/none-done.png';
    },

    useChakra(){
        this.chakra = 0;
    },

    dodge(){
        if(this.stamina<10){
            dialogs.alert("You do not have enough stamina!")
        }else{
        this.stamina = this.stamina - 10;
        }
    },

    rest(){
      
        this.stamina = this.stamina + (this.chakra/2) + 8;
        if(this.stamina > 100){ this.stamina = 100}
        this.chakra = 0;
    },

    useShuriken(){
        if(this.shurikens==0){
            dialogs.alert("You are out of shurikens!")
        }else{
        this.stamina = this.stamina - 2;
        this.shurikens = this.shurikens - 1;
        if(this.shurikens==0){
            this.shurikenPicture = '~/assets/images/none-done.png';
        }
        }
    },

    equipKunai(){
        if(this.kunai==0){
            dialogs.alert("You are out of kunai!")
        }else{
        this.deductKunai();
        this.kunaiEquipped = true; 
        this.kunaiDurability = 6;
        this.kunaiPicture = '~/assets/images/kunai-done.png';
        }
    },

    kunaiBlock(){

        if(this.kunaiDurability==0){
            this.equipKunai()
            return;
        }else{
        this.kunaiDurability --;
        switch(this.kunaiDurability){
            case 6: this.kunaiPicture = '~/assets/images/kunai-done.png'; break;
            case 5: this.kunaiPicture = '~/assets/images/kunai-1d.png'; break;
            case 4: this.kunaiPicture = '~/assets/images/kunai-2d.png'; break;
            case 3: this.kunaiPicture = '~/assets/images/kunai-3d.png'; break;
            case 2: this.kunaiPicture = '~/assets/images/kunai-4d.png'; break;
            case 1: this.kunaiPicture = '~/assets/images/kunai-5d.png'; break;
            case 0: this.kunaiPicture = '~/assets/images/none-done.png'; break;
            
        }
        return;
        }

    },

    addShuriken(){
       
        this.shurikens ++;
        this.shurikenPicture = '~/assets/images/shuriken-throw-done.png';
    },
    deductShuriken(){
          
        this.shurikens --;
    },
    addKunai(){
           
        this.kunai ++;
    },
    deductKunai(){
         
        this.kunai --;
    },



    }
        
    }
    
   
    
  
</script>


<style scoped>

</style>