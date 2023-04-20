<template>
  <base-card>
    <form @submit.prevent="submitForm">
      <div class="form-control">
        <label for="title">Title</label>
        <input v-model="title" type="text" name="title" id="title" />
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea
          v-model="description"
          rows="3"
          name="description"
          id="description"
        />
      </div>
      <div class="form-control">
        <label for="link">Link</label>
        <input v-model="link" type="url" name="link" id="link" />
      </div>
      <div>
        <base-button type="submit">Add Resource</base-button>
      </div>
      <portal v-if="inputIsInvalid" to="body">
        <base-dialog @close="confirmError" title="Invalid input">
          <template #default>
            <p>Unfortunately, at least one input value is invalid</p>
            <p>
              Please check all inputs and make sure you entered at least a few
              characters in each field
            </p>
          </template>
          <template #actions>
            <base-button @click="confirmError"> okay </base-button>
          </template>
        </base-dialog>
      </portal>
    </form>
  </base-card>
</template>

<script>
import BaseDialog from '../UI/BaseDialog.vue';
export default {
  components: { BaseDialog },
  data() {
    return {
      title: '',
      link: '',
      description: '',
      inputIsInvalid: '',
    };
  },
  methods: {
    submitForm() {
      if (
        this.title.trim() === '' ||
        this.description.trim() === '' ||
        this.link.trim() === ''
      ) {
        this.inputIsInvalid = true;
        return;
      }
      this.addResource(this.title, this.description, this.link);
    },
    confirmError() {
      this.inputIsInvalid = false;
    },
  },
  inject: ['addResource'],
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
