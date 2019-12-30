<template>
    <div>
        <ion-header>
            <ion-toolbar>
            <ion-title>
                ionic-app
            </ion-title>
            <ion-buttons slot="end">
            <ion-button @click="goToHome()">Home</ion-button>
            <ion-button @click="goToAbout()">About</ion-button>
            </ion-buttons>
            </ion-toolbar>
        </ion-header>
    </div>
</template>

<script>
export default {
    name: 'myheader',
    methods: {
        goToHome() {
            this.$router.push("/").catch(()=>{});
        },
        goToAbout() {
            this.$router.push("/about").catch(()=>{});
        }
    }
}
</script>

<style>

</style>