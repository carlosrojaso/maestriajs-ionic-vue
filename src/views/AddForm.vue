<template>
<form>
    <ion-item>
    <ion-label position="floating">Name*</ion-label>
    <ion-input
        ref="nameInput"
        :value="newTask.name"
        @input="updateModel()"
        required
    ></ion-input
        >
    <ion-label position="floating">Description*</ion-label>
    <ion-textarea
        ref="descInput"
        :value="newTask.description"
        @input="updateModel()"
        required
    ></ion-textarea>
    <p v-if="errors.length" class="alert-danger">
        <b>Please correct the following error(s):</b>
        <ul>
        <li v-for="error in errors" :key="error">{{ error }}</li>
        </ul>
    </p>
    </ion-item>
    <ion-item>
    <ion-button color="primary"  :disabled="!valid" @click="save()">{{(!isEditing ? 'Add' : 'Edit')}}</ion-button>
    <ion-button color="secondary" @click="cancel()">Cancel</ion-button>
    </ion-item>
</form>  
</template>

<script>
export default {
    name: 'addForm',
    data: function() {
        return {
            newTask: {
                name: '',
                description: ''
            },
            valid: false,
            errors: [],
            isEditing: false
        };
    },
    methods: {
        async cancel() {
            await this.$ionic.modalController.dismiss({action: 'dismiss'});
        },
        async save() {
            if (!this.isEditing) {
            await this.$ionic.modalController.dismiss({action: 'save', result: {name: this.newTask.name, description: this.newTask.description}});
            } else {
            await this.$ionic.modalController.dismiss({action: 'edit', result: {name: this.newTask.name, description: this.newTask.description}});
            }
            this.isEditing = false;
        },
        updateModel() {
            this.newTask.name = this.$refs.nameInput.value;
            this.newTask.description = this.$refs.descInput.value;
            this.checkForm();
        },
        checkForm() {
            this.errors = [];

            if(this.newTask.name.trim().length < 4) {
                this.errors.push('Name must be at least 4 characters.');
            }

            if(this.newTask.description.trim().length < 4) {
                this.errors.push('Description must be at least 4 characters.');
            }

            if(this.newTask.name.trim().length >= 4 && this.newTask.description.trim().length >= 4) {
                this.valid = true;
            } else {
                this.valid = false;
            }
        }
    }
};
</script>

<style>
.alert-danger {
    color: red;
}
</style>