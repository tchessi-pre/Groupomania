<template>
  <div>
    <button
      @click="toggleActions"
      v-if="isAdmin || isCreator"
      :class="customClass"
      class="post-button d-block position-absolute"
      aria-label="Afficher les actions"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="25"
        height="25"
        fill="currentColor"
        class="bi bi-three-dots"
        viewBox="0 0 16 16"
      >
        <path
          d="M3 9.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3zm5 0a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3zm5 0a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3z"
        />
      </svg>
    </button>
    <b-collapse
      v-bind:class="`${
        classCollapse || ''
      } btn-collapsed collapsed mt-2 position-absolute ${
        areActionsVisible && 'visible'
      }`"
    >
      <b-card class="border-0" @click="toggleActions">
        <p class="card-text" v-if="isCreator">
          <b-button
            class="text-left w-100"
            v-if="editingPost && isCreator"
            block
            v-b-modal="`modal-${elementId}`"
            aria-label="Modifier"
          >
            <b-icon icon="pencil" class="mr-2 mr-lg-3"></b-icon
            ><span>{{ modifyText }}</span></b-button
          >
          <b-button
            class="text-left w-100"
            v-if="!editingPost && isCreator"
            block
            @click="clickedEditButton"
            aria-label="Modifier"
          >
            <b-icon icon="pencil" class="mr-2 mr-lg-3"></b-icon
            ><span>{{ modifyText }}</span></b-button
          >
          <slot></slot>
        </p>
        <p class="card-text">
          <b-button
            class="text-left w-100"
            v-if="isAdmin || isCreator"
            block
            v-on:click="onDelete"
            aria-label="Supprimer"
            ><b-icon icon="trash" class="mr-2 mr-lg-3"></b-icon>
            <span>{{ deleteText }}</span></b-button
          >
        </p>
      </b-card>
    </b-collapse>
  </div>
</template>

<script>
import { apiClient } from '../services/apiClient';
export default {
  name: 'EditPost',
  props: [
    'post',
    'customClass',
    'classCollapse',
    'isAdmin',
    'isCreator',
    'elementId',
    'modifyText',
    'deleteText',
    'editingPost',
  ],
  data() {
    return {
      areActionsVisible: false,
    };
  },
  methods: {
    toggleActions() {
      this.areActionsVisible = !this.areActionsVisible;
    },
    async onDelete() {
      this.$emit('onDelete');
    },
    async clickedEditButton() {
      this.$emit('clickedEditButton');
    },
  },
};
</script>

<style lang="scss">
.post-button {
  top: 0px;
  right: 8px;
  margin-top: 15px;
  background-color: transparent;
  border: 0;
  border-radius: 50rem;
  font-size: 0.5rem;
  font-weight: 700;
  color: #747474;
  &:hover {
    outline: none;
    background-color: #17a3b828;
  }
  &:focus {
    outline: none;
  }
}
.btn-secondary {
  font-weight: 500;
  color: #000;
  background-color: white;
  border: none;
}
.btn-outline-secondary,
.btn-secondary {
  &:hover {
    color: #000 !important;
    background-color: rgba(108, 117, 125, 0.1) !important;
    box-shadow: none !important;
  }
}
.btn-collapsed {
  top: 60px;
  right: 15px;
  z-index: 2;
}
.collapsed {
  visibility: hidden;
  display: block !important;
  transform: scaleY(0);
  transform-origin: top;
  transition: transform 0.1s, opacity 0.5s ease-in-out;
  opacity: 0;
  .card-body {
    padding: 1rem;
    box-shadow: 0px 1px 5px 4px rgba(204, 204, 204, 0.2);
  }
}
.visible {
  visibility: visible;
  opacity: 1;
  transform: scaleY(1);
}
@media screen and (min-width: 280px) and (max-width: 767px) {
  .collapsed {
    .card-body {
      padding: 0.5rem;
      box-shadow: 0px 1px 2px 2px rgba(204, 204, 204, 0.2);
    }
  }
  .btn-collapsed {
    top: 28px;
    right: 8px;
  }
  .post-button {
    top: 0px;
    right: 5px;
    font-size: 15px !important;
  }
}
</style>
