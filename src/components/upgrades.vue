<template>
    <div class="upgradeThing" @click="console.log('Nom cliqué :', name); $emit('buy', name)" :class="{darkened: isDarkened}">
        <div>
        <img class="upgradeImage" :src="upgradeData[5]" alt="upgradeIMG">
        </div> 
        <div>
            <h1 class="upgrade">
                {{ name }}
            </h1>
            <h3>
                {{ Math.ceil(upgradeData[1]) }}
            </h3>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Upgrades",
        props: {
            name: String,
            upgradeData: Array,
            isDarkened: Boolean
        }
    }
</script>

<style scoped>
    .upgradeThing {
        position: relative;
        background: linear-gradient(rgb(78, 78, 78), rgba(245, 206, 11, 0.913));
        align-items: center;
        display:flex;
        width: 100%;
        max-height: 100px;
        box-sizing: border-box;    
        border: 3px solid rgba(0, 0, 0, 0.434); 
        cursor: pointer;
    }

    .upgradeThing.darkened {
        filter: brightness(0);
        pointer-events: none;
        transition: filter 0.3s ease;
    }   

    .upgradeImage {
        width: 30%;
        height: 30%;
        margin-left: 20px;
    }
    .upgrades-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
    padding: 12px;
    width: 100%;
    box-sizing: border-box;
    }
</style>