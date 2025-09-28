<template>
    <div class="upgradeThing">
        <div>
        <img class="upgradeImage" src="../image/Ludo.PNG" alt="upgradeIMG">
        </div> 
        <div>
            <div class="upgradeTxt">
                <h1 class="upgrade">
                    Gros caca
                </h1>
                <h2 class="">
                    Quantity
                </h2>
            </div>
            <p>
                petit pipi
            </p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Upgrades"
    }
</script>

<style scoped>
    .upgradeThing {
        position: relative;
        background-color: rgb(124, 121, 121);
        border-radius:10px;
        align-items: center;
        display:flex;
    }

    .upgradeImage {
        width: 50px;
        height: 50px;
        position:relative;
    }

    .upgradeTxt {
        display:flex;
    }
</style>