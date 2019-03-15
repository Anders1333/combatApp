<template>
       <Page>
	<ActionBar >
         <Label color="black" fontSize="26" alignText="center" text="Dice Roll Calculator"/>
  </ActionBar>
<StackLayout>

        <StackLayout paddingTop="30px" class="diceHolder" orientation="horizontal">
         <ListPicker  class="dicePicker" :items="diceAmounts" v-model="selectedAmount" ></ListPicker> 
         <Label marginTop = "80px" paddingLeft = "20px" text="X"/>
         <ListPicker class="dicePicker"  :items="dicetypes" v-model="selectedType" ></ListPicker> 
        </StackLayout>
   
        <Button marginTop="20px" height="100px" width="200px" text="Calculate" @tap="calculate"/>
        <StackLayout paddingBottom="20px" orientation="horizontal">
          <Label text="Value"/>
          <Label marginLeft="10px" text="Chance"/>
          <Label text="Average:" marginLeft="120px"/>
          <Label :text="this.average" marginLeft="20px"/>
        </StackLayout>   

      
        <ScrollView height="2000px">

        <GridLayout width="auto" height="auto" columns= "auto,auto,auto,atuo" rows="auto,auto,auto,auto">
          <ListView col="0" row="0" for="value in allOcurrences" width = "85px" height="1600px">
              <v-template>
    <!-- Shows the list item label in the default color and style. -->
                 <StackLayout orientation="horizontal">
                 <Label :text="value" width="90px" />
              
                 </StackLayout>
                 
              </v-template>
          </ListView>  
         <ListView col="1" row="0" for="value in allChances" width = "auto" height="1600px">
              <v-template>
    <!-- Shows the list item label in the default color and style. -->
                 <StackLayout orientation="horizontal">
                 <Label :text="value" width="100px" />
                 <Label text="%"/>
                 </StackLayout>
                 
              </v-template>
          </ListView>  
           <ListView col="2" row="0" for="value in allChances" width = "1200px" height="1600px">
              <v-template>
    <!-- Shows the list item label in the default color and style. -->
                 <StackLayout orientation="horizontal">
                 <Label backgroundColor="blue" :width="value*10" />
              
                 </StackLayout>
                 
              </v-template>
          </ListView>  
      
        </GridLayout>
      

       </ScrollView>
 


       

  
   

</StackLayout>

</Page>
</template>




<script >
import { ListPicker } from "tns-core-modules/ui/list-picker";

  export default {
    data() {
      return {
        dicetypes: ["d4","d6","d8","d12","d20"],
        diceAmounts: [1,2,3,4,5,6,7,8,9],
        selectedAmount: 0,
        selectedType: 0,
        currentMax: 24,
        allOcurrences: [],
        allChances: [],
        average: 0,
        ocurrences: [],
        ocurrencesAll: [],
       
        

      }
    },
    methods: {

    goBack() {
     
    },
    clearTable(){
    this.numberOfDice = 0;
    this.allOcurrences = [];
    this.allChances= [];
    this.ocurrences = [];
  
    },
    calculate(){
    this.clearTable()
    this.defineChoice(this.selectedAmount,this.selectedType)

    console.log(this.selectedAmount + " ," + this.selectedType)
    let numberOfDice = this.selectedAmount ; 
    let diceMax = this.selectedType;

    let dice = [];


    for(let i=1; i <= diceMax ; i++){
             dice.push(i)
    }

console.log(dice);
this.average = ((diceMax+1)/2) * numberOfDice;
    let outcomeArray = this.rollDice(dice,numberOfDice);


    outcomeArray.sort(this.sortNumber);
    let numberOfOutcomes = outcomeArray.length;
  
    let ocurrences = this.countOcurrences(outcomeArray);
  
    for(let z=0; z < ocurrences[0].length; z++){
    console.log("V: " + ocurrences[0][z] + " O: " + ocurrences[1][z] + " Chance: " +(100/numberOfOutcomes)*ocurrences[1][z] + " %")
   
    }

    this.ocurrencesAll = ocurrences[1];
   
     this.allOcurrences = ocurrences[0];
    for(let i=0;i<ocurrences[1].length; i++){
         this.allChances.push((100/numberOfOutcomes)*ocurrences[1][i]); 
    }
    this.selectedAmount = 0;
    this.selectedType = 0;



   

   
    },

    defineChoice(amount,type){
     this.selectedAmount = amount + 1;

     switch(type){
       case 0: this.selectedType = 4; return;
       case 1: this.selectedType = 6; return;
       case 2: this.selectedType = 8; return;
       case 3: this.selectedType = 12; return;
       case 4: this.selectedType = 20; return;
     }
    },
    
    
    countOcurrences(arr) {
    var a = [], b = [], prev;

    
    for ( var i = 0; i < arr.length; i++ ) {
        if ( arr[i] !== prev ) {
            a.push(arr[i]);
            b.push(1);
        } else {
            b[b.length-1]++;
        }
        prev = arr[i];
    }

    return [a,b];
},

rollDice(dice,amount){  

  let outcomes = [];
      switch(amount){
      case 1:  return dice;
      case 2:  for(let i=0; i < dice.length; i++){
                     for(let j = 0; j<dice.length; j++){
                        outcomes.push(dice[i]+dice[j])
                     }
                }   return outcomes;
      case 3:  for(let i=0; i < dice.length; i++){
                     for(let j = 0; j<dice.length; j++){
                         for(let k = 0; k<dice.length; k++){
                           outcomes.push(dice[i]+dice[j]+dice[k])
                         }
                     }
                }   return outcomes;
      case 4: for(let i=0; i < dice.length; i++){
                        for(let j = 0; j<dice.length; j++){
                          for(let k = 0; k<dice.length; k++){
                            for(let l = 0; l<dice.length; l++){
                              outcomes.push(dice[i]+dice[j]+dice[k]+dice[l])
                            }
                          }
                        }
                      }  return outcomes;
      case 5: for(let i=0; i < dice.length; i++){
                        for(let j = 0; j<dice.length; j++){
                          for(let k = 0; k<dice.length; k++){
                            for(let l = 0; l<dice.length; l++){
                              for(let m = 0; m<dice.length; m++){
                                 outcomes.push(dice[i]+dice[j]+dice[k]+dice[l]+dice[m])
                              }
                            }
                          }
                        } 
                      } return outcomes;
      case 6: for(let i=0; i < dice.length; i++){
                        for(let j = 0; j<dice.length; j++){
                          for(let k = 0; k<dice.length; k++){
                            for(let l = 0; l<dice.length; l++){
                              for(let m = 0; m<dice.length; m++){
                                for(let n = 0; n<dice.length; n++){
                                   outcomes.push(dice[i]+dice[j]+dice[k]+dice[l]+dice[m]+dice[n])
                                }                               
                              }
                            }
                          }
                        } 
                      } return outcomes;
      case 7: for(let i=0; i < dice.length; i++){
                        for(let j = 0; j<dice.length; j++){
                          for(let k = 0; k<dice.length; k++){
                            for(let l = 0; l<dice.length; l++){
                              for(let m = 0; m<dice.length; m++){
                                for(let n = 0; n<dice.length; n++){
                                  for(let o = 0; o<dice.length; o++){
                                   outcomes.push(dice[i]+dice[j]+dice[k]+dice[l]+dice[m]+dice[n]+dice[o])
                                  }
                                }                               
                              }
                            }
                          }
                        } 
                      } return outcomes;
      case 8: for(let i=0; i < dice.length; i++){
                        for(let j = 0; j<dice.length; j++){
                          for(let k = 0; k<dice.length; k++){
                            for(let l = 0; l<dice.length; l++){
                              for(let m = 0; m<dice.length; m++){
                                for(let n = 0; n<dice.length; n++){
                                  for(let o = 0; o<dice.length; o++){
                                     for(let p = 0; p<dice.length; p++){
                                   outcomes.push(dice[i]+dice[j]+dice[k]+dice[l]+dice[m]+dice[n]+dice[o]+dice[p])
                                     }
                                  }
                                }                               
                              }
                            }
                          }
                        } 
                      } return outcomes;
      } 
},

sortNumber(a,b){
  return a-b;
},
    

    
    }
  }
</script>

<style scoped lang="scss">
@import "../app.scss";
   
</style>