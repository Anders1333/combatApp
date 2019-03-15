<template>
    <Page>
        <ActionBar title="Adventure Helper (alpha)"/>
       
        <GridLayout paddingTop="200px" paddingLeft="200px"  columns="auto,auto,auto,auto" rows="auto,auto,auto,auto">
            <Label text="Menu" col="0" row="0"/>
            <Button text="Combat Tracker" @tap= "goToCombat" height="80px" width="auto" col="0" row="1"/>
            <Button text="Dice Calculator" @tap= "goToDice" height="80px" width="auto" col="0" row="2"/>
        </GridLayout>
    </Page>
</template>

<script >
import AnotherPage from './AnotherPage.vue';
import DicePage from './DicePage.vue';
  export default {
    data() { 
      return {
        msg: 'Hello World!'
      }
    },
    methods: {
    goToCombat() {
      this.$navigateTo(AnotherPage);
    },

    goToDice(){
      this.$navigateTo(DicePage);
    }
    }
  }
</script>

<style scoped lang="scss">
@import "../app.scss";
   
</style>
