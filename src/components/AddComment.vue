<template>
  <div class="form-wrapper">
    <div class="author" username="username">
      <img :src="avatar" alt="" class="avatar" />
    </div>
    <form class="add-form" @submit.prevent="$emit('submitForm', value)">
      <textarea
        class="comment-text"
        placeholder="Add a comment..."
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
      ></textarea>
      <button class="add-button">Send</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'AddComment',
  props: {
    avatar: String,
    username: String,
    modelValue: String,
    isReply: Boolean
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;

.form-wrapper {
  display: flex;
  align-items: flex-start;
  width: 100%;
  background: $white;
  padding: 20px 28px;
  border-radius: 10px;
  margin-top: 20px;
  @media (max-width: 550px) {
    flex-direction: column-reverse;
    padding: 10px 14px;
  }

  .avatar {
    max-width: 30px;
  }

  .add-form {
    width: 100%;
    display: flex;
    @media (max-width: 550px) {
      flex-direction: column;
    }
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
    @media (max-width: 550px) {
      margin: 0;
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
    @media (max-width: 550px) {
      margin-left: auto;
      margin-top: 20px;
    }
  }
}
</style>
