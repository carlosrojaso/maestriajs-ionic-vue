<template>
  <div class="ion-page">
    <Myheader/>
    <ion-content class="ion-padding">
    <h1>This is an about page</h1>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent sed consequat massa. Aenean vitae orci fermentum, tristique odio et, pulvinar tortor. Sed pellentesque lacus ligula, ac dignissim sapien rhoncus in. Donec convallis semper nulla vitae venenatis. Sed sed gravida magna. Vestibulum in ex facilisis, auctor leo non, dictum magna. Proin imperdiet mollis pharetra.</p>
    </ion-content>
    <Myfooter/>
  </div>
</template>

<script>
import Myheader from "./Header";
import Myfooter from "./Footer";

export default {
  name: 'About',
  components: {
    Myheader,
    Myfooter
  }
}
</script>

<style>

</style>

