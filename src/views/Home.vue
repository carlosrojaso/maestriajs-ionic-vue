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

import {notesDataApi} from "../data/notes-data-api";

export default {
  name: "home",
  mixins: [notesDataApi],
  data: function() {
    return {
      notes: [],
      isEditing: false,
      idToEdit: -1
    };
  },
  components: {
    Myheader,
    Myfooter
  },
  mounted() {
    this.getTasks(1)
      .then((res) => res.json())
      .then((items) => {
        this.notes = items.map((item) => ({id: item.id, name: item.title, description: item.body}));
        });
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
  
      this.createTask(newNote)
        .then(res => res.json())
        .then(
          () => {
            this.notes.push(newNote);
          }
        );
    },
    deleteNote(id) {
      const noteToDelete = this.notes.findIndex((item) => (item.id === id));
      this.deleteTask(id)
        .then(res => res.json())
        .then(
          () => {
            this.notes.splice(noteToDelete, 1);
          }
        );
    },
    editNote(note) {
      const originalNote = this.notes.findIndex((item) => (item.id === this.idToEdit));
      const noteToEdit = {
          id: this.idToEdit,
          name: note.name,
          description: note.description
          }; 
      
      this.putTask(noteToEdit)
          .then(res => res.json())
          .then(
            () => {
              this.notes[originalNote] = {...noteToEdit};
              this.hackRender();
            }
          );
    },
    hackRender() {
      /**fix to update list **/
      this.notes.push('');
      this.notes.pop();
    }
  }
};
</script>