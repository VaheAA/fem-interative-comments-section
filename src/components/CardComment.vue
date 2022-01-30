<template>
  <div class="comment">
    <p class="comment__text">{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: 'CardComment',
  props: {
    text: String
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;
.comment {
  &__text {
    color: $grayishBlue;
    font-size: 16px;
  }
}
</style>
