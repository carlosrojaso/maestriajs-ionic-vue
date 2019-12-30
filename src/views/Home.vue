<template>
  <div class="ion-page">
    <Myheader/>
    <ion-content class="ion-padding">
    <ion-list v-for="note in notes" :key="note.id">
      <ion-item-sliding>
        <ion-item>
          <ion-text>
          <h3>{{note.name}}</h3>
          <p>{{note.description}}</p>
        </ion-text>
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
import AddForm from "./AddForm";

import { DummyData } from "../data/dummy-data";

export default {
  name: "home",
  data: function() {
    return {
      notes: DummyData,
      isEditing: false,
      idToEdit: -1
    };
  },
  components: {
    Myheader,
    Myfooter
  },
  methods: {
    async openDialog(id) {
      let modalProps;
      if(id) {
        this.isEditing = true;
      }

      if(!id) {
        modalProps = {
          component: AddForm
        };
      } else {
        const noteToEdit = this.notes.findIndex((item) => (item.id === id));
        this.idToEdit = id;
        modalProps = {
          component: AddForm,
          componentProps: {
            data: {
              newTask: {
                name: this.notes[noteToEdit].name,
                description: this.notes[noteToEdit].description
              },
              isEditing: true,
              valid: true
            }
          }
        };
      }

      this.modal = await this.$ionic.modalController.create(modalProps);

      this.modal.present();
      const { data } = await this.modal.onWillDismiss();

      switch (data.action) {
        case 'save':
          this.saveNote(data.result);
          break;
        case 'edit':
          this.editNote(data.result);
          break;
        case 'dismiss':
          break;
      }
    },
    saveNote(note) {
      const newIndex = this.notes.length + 1;
      const newNote = {
        id: newIndex,
        name: note.name,
        description: note.description
      };
      this.notes.push(newNote);
    },
    deleteNote(id) {
      const noteToDelete = this.notes.findIndex((item) => (item.id === id));
      this.notes.splice(noteToDelete, 1);
    },
    editNote(note) {
      const originalNote = this.notes.findIndex((item) => (item.id === this.idToEdit));
      const noteToEdit = {
          id: this.idToEdit,
          name: note.name,
          description: note.description
          }; 
      this.notes[originalNote] = {...noteToEdit};
      /**fix to update**/
      this.notes.push('');
      this.notes.pop();
    },
    doRefresh(event) {

      setTimeout(() => {
        event.target.complete();
      }, 2000);
    }
  }
};
</script>