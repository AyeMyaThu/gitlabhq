<script>
import Flash from '../../../flash';
import editForm from './edit_form.vue';

export default {
  components: {
    editForm,
  },
  props: {
    isConfidential: {
      required: true,
      type: Boolean,
    },
    isEditable: {
      required: true,
      type: Boolean,
    },
    service: {
      required: true,
      type: Object,
    },
  },
  data() {
    return {
      edit: false,
    };
  },
  computed: {
    faEye() {
      const eye = this.isConfidential ? 'fa-eye-slash' : 'fa-eye';
      return {
        [eye]: true,
      };
    },
  },
  methods: {
    toggleForm() {
      this.edit = !this.edit;
    },
    updateConfidentialAttribute(confidential) {
      this.service.update('issue', { confidential })
        .then(() => location.reload())
        .catch(() => new Flash('Something went wrong trying to change the confidentiality of this issue'));
    },
  },
};
</script>

<template>
  <div class="block issuable-sidebar-item confidentiality">
    <div class="sidebar-collapsed-icon">
      <i class="fa" :class="faEye" aria-hidden="true"></i>
    </div>
    <div class="title hide-collapsed">
      Confidentiality
      <a
        v-if="isEditable"
        class="pull-right confidential-edit"
        href="#"
        @click.prevent="toggleForm"
      >
        Edit
      </a>
    </div>
    <div class="value sidebar-item-value hide-collapsed">
      <editForm
        v-if="edit"
        :toggle-form="toggleForm"
        :is-confidential="isConfidential"
        :update-confidential-attribute="updateConfidentialAttribute"
      />
      <div v-if="!isConfidential" class="no-value sidebar-item-value">
        <i class="fa fa-eye sidebar-item-icon"></i>
        Not confidential
      </div>
      <div v-else class="value sidebar-item-value hide-collapsed">
        <i aria-hidden="true" class="fa fa-eye-slash sidebar-item-icon is-active"></i>
        This issue is confidential
      </div>
    </div>
  </div>
</template>
