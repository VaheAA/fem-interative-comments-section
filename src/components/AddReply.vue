<template>
  <div class="reply-wrapper" :isActive="isActive">
    <div class="author" username="username">
      <img :src="avatar" alt="" class="avatar" />
    </div>
    <form class="add-form" @submit.prevent="$emit('submitReply', value)">
      <textarea
        class="comment-text"
        placeholder="Add a comment..."
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
      ></textarea>
      <button class="add-button">Reply</button>
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
    isActive: Boolean
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;

.reply-wrapper {
  display: flex;
  align-items: flex-start;
  width: 100%;
  max-width: 600px;
  margin-left: auto;
  background: $white;
  padding: 20px 28px;
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
</style>
