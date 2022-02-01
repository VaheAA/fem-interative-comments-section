<template>
  <div class="likes">
    <button @click="addLikes" class="like-btn like-btn--add">+</button>
    <span class="likes-count">{{ likes }}</span>
    <button @click="removeLikes" class="like-btn like-btn--remove">-</button>
  </div>
</template>

<script>
export default {
  name: 'Likes',
  props: {
    likes: Number
  },
  methods: {
    addLikes() {
      this.$emit('addLikes');
    },
    removeLikes() {
      this.$emit('removeLikes');
    }
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;
.likes {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 40px;
  padding: 5px 15px;
  background-color: $lightGray;
  border-radius: 10px;
  max-height: 100px;

  @media (max-width: 550px) {
    flex-direction: row;

    max-width: 100px;
    max-height: 40px;
    margin-top: 15px;
  }

  .like-btn {
    background: none;
    border: none;
    color: $lightGrayishBlue;
    font-size: 22px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.2s ease;
    &:first-child {
      margin-bottom: 10px;
      @media (max-width: 550px) {
        margin-bottom: 0;
        marig-right: 5px;
      }
    }
    &:last-child {
      margin-top: 10px;
      margin-bottom: 10px;
      @media (max-width: 550px) {
        margin-bottom: 0;
        margin-top: 0;
        margin-left: 5px;
      }
    }
    &:hover {
      color: $darkBlue;
    }
  }

  .likes-count {
    font-weight: 500;
    color: $modBlue;
  }
}
</style>
