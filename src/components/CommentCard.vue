<template>
  <div class="card" :reply="reply">
    <div class="card__likes">
      <Likes :likes="likes" @addLikes="addLikes" @removeLikes="removeLikes" />
    </div>
    <div class="card__content">
      <CardHeader :avatar="avatar" :username="username" :date="date" />
      <CardComment :text="text" />
    </div>
  </div>
</template>

<script>
import Likes from './Likes.vue';
import CardHeader from './CardHeader.vue';
import CardComment from './CardComment.vue';
export default {
  name: 'CommentCard',
  components: {Likes, CardHeader, CardComment},
  props: {
    reply: Boolean,
    likes: Number,
    avatar: String,
    username: String,
    date: String,
    text: String
  },
  data() {
    return {
      nestedCard: false
    };
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
.card {
  background-color: $white;
  color: $darkBlue;
  padding: 20px 28px;
  border-radius: 10px;
  display: flex;
  align-items: flex-start;
  &:not(:last-child) {
    margin-bottom: 20px;
  }

  &__content {
    width: 100%;
    margin-left: 20px;
  }
}
</style>
