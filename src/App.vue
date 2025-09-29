<script>
import FeatherImg from './image/Plume.png'
import ShadeImg from './image/Shade.png' 
import GhostImg from './image/Scribe.png'
import ScribeImg from './image/GhostWriter.png'

import plumeUpImg from './image/PlumeUpdate.png'
import shadeUpImg from './image/shadesUpdate.png'
import ghostUpImg from './image/GhostUpdate.png'
import scribeUpImg from './image/ScribeUpdate.png'


export default {
  name: "CookieClicker",
  data() {
    return {
      numberPartitions: 0,
      partitionsPerSecond: 0,
      ListAchievements: {
        "Feather": false,
        "Shade": false,
        "GhostWriter": false,
        "Scribe": false,
        "FeatherUpd": false,
        "ShadeUpd": false,
        "GhostWriterUpd": false,
        "ScribeUpd": false,
        "AllDone": false
      },

      ListImprovements: {
        // [0] -> Quantity, [1] -> Current Price (ROUND UP!!!), [2] -> Price acceleration rate (Doesnt Change)
        // [3] -> Multiplicators, [4] -> +10%boost, [5] -> src
        "Feather": [1, 10, 1.5, 1, 0, FeatherImg, 100], 
        "Shade": [0, 15, 1.2, 0.1, 0, ShadeImg, 150],
        "GhostWriter": [0, 100, 1.15, 2, 0, GhostImg, 500],
        "Scribe": [0, 1000, 1.15, 9, 0, ScribeImg, 10000]
      },

      Interval: 0
    };
  },

  mounted() { 

    if (localStorage.numberPartitions) {
      this.numberPartitions = parseInt(localStorage.numberPartitions, 10);
    }

    if (localStorage.partitionsPerSecond) {
      this.partitionsPerSecond = parseInt(localStorage.partitionsPerSecond, 10);
    }

    if (localStorage.ListAchievements) {
      this.ListAchievements = JSON.parse(localStorage.ListAchievements);
    }

    if (localStorage.ListImprovements) {
      this.ListImprovements = JSON.parse(localStorage.ListImprovements);
    }
        
    this.Interval = setInterval(() => {
        ["Shade", "GhostWriter", "Scribe"].forEach(key => {
            const imp = this.ListImprovements[key];
            if (imp) this.addPartition(imp);
        });
    }, 500);
  },
  beforeUnmount() {
    clearInterval(this.Interval);
  },

  watch: {
    numberPartitions(newValue) {
      localStorage.numberPartitions = newValue;
    },
    partitionsPerSecond(newValue) {
      localStorage.partitionsPerSecond = newValue;
    },
    ListAchievements: {
      deep: true,
      handler(newValue){
        localStorage.ListAchievements = JSON.stringify(newValue);
      }
    },
    ListImprovements: {
      deep: true,
      handler(newValue){
        localStorage.ListImprovements = JSON.stringify(newValue);
      }
    }
  },

  methods: {
    addPartition(selectedAdder) {
      if (selectedAdder[4] > 0){
        this.numberPartitions += Number(selectedAdder[0]) * Number(selectedAdder[3]) * 1.1;
      }else{
        this.numberPartitions += Number(selectedAdder[0]) * Number(selectedAdder[3]);
      }

      if (this.numberPartitions >= 10 && !this.ListAchievements["Feather"]){
        alert("ACHIEVEMENT:\nYou can now buy new feathers. \nIt should be much easier to write now...");
        this.ListAchievements["Feather"] = true;
      }
      if (this.numberPartitions >= 15 && !this.ListAchievements["Shade"]){
        alert("ACHIEVEMENT:\nYou can now buy Shades. \nPlease treat them well.");
        this.ListAchievements["Shade"] = true;
      }
      if (this.numberPartitions >= 100 && !this.ListAchievements["GhostWriter"]){
        alert("ACHIEVEMENT:\nYou can now buy Ghost Writers. \nThey are a bit snobbish at times but they work well.");
        this.ListAchievements["GhostWriter"] = true;
      }
      if (this.numberPartitions >= 1000 && !this.ListAchievements["Scribe"]){
        alert("ACHIEVEMENT:\nYou can now buy Scribes. \nWhat pretty flowers.");
        this.ListAchievements["Scribe"] = true;
      }
      if (this.numberPartitions >= 100 && !this.ListAchievements["FeatherUpd"] && this.ListImprovements['Feather'][0] >= 2){
        alert("ACHIEVEMENT:\nHigher quality feathers arrived! \nFeel free to exchange your partitions for them.\n(Buy for a +10% income buff)");
        this.ListAchievements["FeatherUpd"] = true;
      }
      if (this.numberPartitions >= 150 && !this.ListAchievements["ShadeUpd"]  && this.ListImprovements['Shade'][0] >= 5){
        alert("ACHIEVEMENT:\nIt seems your shades want to work harder! \nWell done!");
        this.ListAchievements["ShadeUpd"] = true;
      }
      if (this.numberPartitions >= 500 && !this.ListAchievements["GhostWriterUpd"] && this.ListImprovements['GhostWriter'][0] >= 10){
        alert("ACHIEVEMENT:\nMotivating ghosts to work is no easy task.\nKeep going!");
        this.ListAchievements["GhostWriterUpd"] = true;
      }
      if (this.numberPartitions >= 10000 && !this.ListAchievements["ScribeUpd"] && this.ListImprovements['Scribe'][0] >= 50){
        alert("ACHIEVEMENT:\nYou're almost there.\nCongratulations.");
        this.ListAchievements["ScribeUpd"] = true;
      }
    },
    buyUpgrades(upgrade) {
      if(this.numberPartitions >= Number(this.ListImprovements[upgrade][1])){
        this.ListImprovements[upgrade][0]++;
        this.numberPartitions -= this.ListImprovements[upgrade][1];
        this.ListImprovements[upgrade][1] *= this.ListImprovements[upgrade][2];
        if (upgrade != "Feather"){
          this.partitionsPerSecond += Number(this.ListImprovements[upgrade][0]) * Number(this.ListImprovements[upgrade][3]);
        }
      }
    },
    buyImprovement(upgrade) {
      if(this.numberPartitions >= Number(this.ListImprovements[upgrade][6]) && this.ListImprovements[upgrade][4] < 1){
        this.ListImprovements[upgrade][4] = 1;
      }
    },
    reset() {
      this.numberPartitions = 0;
      this.ListAchievements["Feather"] = false;
      this.ListAchievements["GhostWriter"] = false;
      this.ListAchievements["Shade"] = false;
      this.ListAchievements["Scribe"] = false;
    }
  }
  
};
</script>

<script setup>
import PartitionButton from "./components/partitionClicker.vue"
import upgrades from "./components/upgrades.vue";
import upgradesUpgrades from "./components/upgradesUpgrades.vue";
</script>

<template>
  
  <div id="app">
    <div class="leftBlock">
      <img class="clickUpdate" v-if="ListAchievements['FeatherUpd']" :src="plumeUpImg" @click="buyImprovement('Feather')" />
      <img class="clickUpdate" v-if="ListAchievements['ShadeUpd']" :src="shadeUpImg" @click="buyImprovement('Shade')"/>
      <img class="clickUpdate" v-if="ListAchievements['GhostWriterUpd']" :src="ghostUpImg" @click="buyImprovement('GhostWriter')"/>
      <img class="clickUpdate" v-if="ListAchievements['ScribeUpd']" :src="scribeUpImg" @click="buyImprovement('Scribe')"/>
    </div>
    <div class="centerBlock">
      <p class="Partition">
        <PartitionButton @clicked="addPartition(ListImprovements['Feather'])" />
        You wrote {{ Math.ceil(numberPartitions) }} partitions in recent memory.
        <br>Your writers write {{ Math.ceil(partitionsPerSecond) }} partitions per seconds. 
      </p>
    </div>
    <div class="rightBlock">
        <upgradesUpgrades />

      <div class="rightBlockBody">
        <upgrades 
          v-for="(upgrade, name) in ListImprovements"
          :key="name"
          :name="name"
          :upgradeData="upgrade"
          @buy="buyUpgrades($event)"
          :is-darkened="!ListAchievements[name]"
          />
      </div>
    </div>
  </div>
</template>

<style scoped>
  htlm, body {
    margin: 0;
    padding: 0;
    height: 100%;
  }

  #app {
    display: flex;
    height: 100vh;
    background-color: black;
  }

  .leftBlock {
    flex: 0 0 25%;
    background-color: rgb(14, 14, 14);
    position: relative;
    display:flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .centerBlock {
    flex:0 0 45%;
    background:linear-gradient(to left, rgb(14, 14, 14), brown, rgb(14, 14, 14));
    position:relative;
    text-align: center;
  }

  .rightBlock {
    flex:0 0 30%;
    background-color: rgb(14, 14, 14);
    position:relative;  
  }

  .rightBlockBody {
    align-items: center;
    display: flex;
    flex-direction: column;
  }

  .Partition {
    position: absolute;
    top:50%;
    left:50%;
  transform: translate(-50%, -50%);
  }

  .clickUpdate {
  width: 100px;
  height: 100px;
  display: block;
  margin: 10px;
  background-color: rgb(214, 182, 0);
  border: 5px solid rgb(251, 255, 0);
  }
</style>
