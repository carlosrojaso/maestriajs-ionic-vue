<template>
    <div>
        <ion-header>
            <ion-toolbar>
            <ion-title>
                ionic-app
            </ion-title>
            <ion-buttons slot="end">
            <ion-button :routerLink="['/']">Home</ion-button>
            <ion-button :routerLink="['/about']">About</ion-button>
            </ion-buttons>
            </ion-toolbar>
        </ion-header>
    </div>
</template>

<script>
export default {
    name: 'myheader'
}
</script>

<style>

</style>