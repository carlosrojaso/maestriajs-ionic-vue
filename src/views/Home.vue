<template>
  <div class="ion-page">
    <Myheader/>
    <ion-content class="ion-padding">
    <ion-list v-for="note in notes" :key="note.id">
      <ion-item-sliding>
        <ion-item>
          <ion-label>{{note.name}}</ion-label>
        </ion-item>
        <ion-item-options side="end">
          <ion-item-option @click="openDialog(note.id)">Edit</ion-item-option>
          <ion-item-option color="warning" @click="deleteNote(note.id)">Delete</ion-item-option>
        </ion-item-options>
      </ion-item-sliding>
    </ion-list>
    </ion-content>
    <ion-fab vertical="bottom" horizontal="end" slot="fixed">
      <ion-fab-button @click="openDialog()">
        <ion-icon name="add"></ion-icon>
      </ion-fab-button>
    </ion-fab>
    <Myfooter/>
  </div>
</template>

<script>
import Myheader from "./Header";
import Myfooter from "./Footer";

import { DummyData } from "../data/dummy-data";

export default {
  name: "home",
  data: function() {
    return {
      notes: DummyData
    };
  },
  components: {
    Myheader,
    Myfooter
  }
};
</script>