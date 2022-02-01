<template>
  <div class="modal" v-if="isOpen">
    <div class="modal-content">
      <h4 class="modal-title">Delete comment</h4>
      <p class="modal-text">
        Are you sure you want to delete this comment? This can't be undone
      </p>
      <div class="modal-cta">
        <button class="btn btn-negative" @click="closeModal">No, Cancel</button>
        <button class="btn btn-positive" @click="deleteComment">
          Yes, Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    isOpen: Boolean
  },
  methods: {
    closeModal() {
      this.$emit('closeModal');
    },
    deleteComment() {
      this.$emit('deleteComment');
    }
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;

.modal {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba($color: #000000, $alpha: 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;

  &-content {
    background: #fff;
    max-width: 400px;
    width: 90%;
    padding: 25px;
    border-radius: 10px;
    margin: 0 auto;
  }
  &-title {
    color: $grayishBlue;
    font-weight: bold;
    font-size: 24px;
    margin-bottom: 15px;
  }
  &-text {
    color: $grayishBlue;
    margin-bottom: 15px;
  }
  &-cta {
    display: flex;
    justify-content: center;
    width: 100%;
    gap: 15px;
  }
  .btn {
    border: none;
    width: 100%;
    padding: 15px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
    font-weight: bold;
    color: $white;
    &-negative {
      background: $grayishBlue;
    }
    &-positive {
      background: $softRed;
    }
  }
}
</style>
