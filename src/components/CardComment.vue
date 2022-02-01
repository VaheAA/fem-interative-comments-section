<template>
  <div class="comment">
    <p class="comment__text" :isEditable="isEditable" v-if="!isEditable">
      {{ text }}
    </p>
    <div class="update-wrapper" :isEditable="isEditable" v-if="isEditable">
      <div class="author" username="username">
        <img :src="avatar" alt="" class="avatar" />
      </div>
      <form class="add-form" @submit.prevent="$emit('submitUpdate', value)">
        <textarea
          class="comment-text"
          placeholder="Add a comment..."
          :value="modelValue"
          @input="$emit('update:modelValue', $event.target.value)"
        ></textarea>
        <button class="add-button">Update</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardComment',
  props: {
    text: String,
    avatar: String,
    username: String,
    modelValue: String,
    isEditable: Boolean
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;
.comment {
  &__text {
    color: $grayishBlue;
    font-size: 16px;

    @media (max-width: 550px) {
      font-size: 14px;
    }
  }
  .update-wrapper {
    display: flex;
    align-items: flex-start;
    width: 100%;
    background: $white;
    border-radius: 10px;
    margin-bottom: 20px;

    .avatar {
      max-width: 30px;
    }

    .add-form {
      width: 100%;
      display: flex;
    }
    .comment-text {
      width: 100%;
      outline: none;
      margin: 0 15px;
      padding: 15px 20px;
      border-radius: 10px;
      border: 1px solid rgba($color: $darkBlue, $alpha: 0.2);
      resize: none;
      font-family: inherit;
      color: $grayishBlue;
      &:focus {
        border: 1px solid rgba($color: $darkBlue, $alpha: 0.7);
      }
    }
    .add-button {
      background: $modBlue;
      border: none;
      color: $white;
      border-radius: 5px;
      width: 100px;
      height: 40px;
      text-transform: uppercase;
      font-weight: bold;
      transition: all 0.2s ease;
      cursor: pointer;

      &:hover {
        opacity: 0.5;
      }
    }
  }
}
</style>
