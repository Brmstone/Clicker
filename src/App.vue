<script>
export default {
  name: "CookieClicker",
  data() {
    return {
      numberPartitions: 0,
      ListAchievements: {
        "1er": false
      },

      ListImprovements: {
        // [0] -> Quantity, [1] -> Current Price (ROUND UP!!!), [2] -> Price acceleration rate (Doesnt Change)
        // [3] -> Multiplicators
        "ClickBase": [1, 10, 1.5, 1], 
        "GhostWriter": [0, 50, 1.33, 1]
      },

      Interval: 0
    };
  },

  mounted() { 

    if (localStorage.numberPartitions) {
      this.numberPartitions = parseInt(localStorage.numberPartitions, 10);
    }

    if (localStorage.ListAchievements) {
      this.ListAchievements = JSON.parse(localStorage.ListAchievements);
    }

    if (localStorage.ListImprovements) {
      this.ListImprovements = JSON.parse(localStorage.ListImprovements);
    }
    
    this.Interval = setInterval(() => {
      this.addPartition(this.ListImprovements["GhostWriter"])
    },
    500)
  },

  watch: {
    numberPartitions(newValue) {
      localStorage.numberPartitions = newValue;
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
      this.numberPartitions += Number(selectedAdder[0]) * Number(selectedAdder[3]);


      if (this.numberPartitions == 155 && !this.ListAchievements["1er"]){
        alert("1er Achèvement");
        this.ListAchievements["1er"] = true;
      }
    },
    buyUpgrades(upgrade) {
      if(this.numberPartitions > Number(upgrade[1])){
        upgrade[0]++;
        this.numberPartitions -= upgrade[1];
      }
    },
    reset() {
      this.numberPartitions = 0;
      this.ListImprovements['GhostWriter'][0] = 0;
    }
  }
  
};
</script>

<script setup>
import PartitionButton from "./components/partitionClicker.vue"
import upgrades from "./components/upgrades.vue";
//,
    //Interval: setInterval(100, addPartition(this.ListImprovements[1]))
    
//Interval: setInterval(100, addPartition(this.ListImprovements[1]))
</script>

<template>
  
  <div id="app">
    <div class="leftBlock">
      Spectators
      <p class="Partition">
        <PartitionButton @clicked="numberPartitions--" />
      </p>
    </div>
    <div class="centerBlock">
      <p class="Partition">
        <PartitionButton @clicked="addPartition(ListImprovements['ClickBase'])" />
        I have {{ numberPartitions }} partitions 
      </p>
    </div>
    <div class="rightBlock">
      <p>
        <button @click="buyUpgrades(ListImprovements['GhostWriter'])"> Text </button>
        <button @click="reset()"> Reset </button>
      </p>
      <div class="rightBlockBody">
        <p>
          <upgrades />
        </p>
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
  }

  .leftBlock {
    flex: 0 0 25%;
    background-color: aquamarine;
    position: relative;
  }

  .centerBlock {
    flex:0 0 45%;
    background-color:brown;
    position:relative;
  }

  .rightBlock {
    flex:0 0 30%;
    background-color: blue;
    position:relative;
  }

  .rightBlockBody {
    align-items: center;
    display: flex;
  }

  .Partition {
    position: absolute;
    top:50%;
    left:50%;
  transform: translate(-50%, -50%);
  }

</style>
