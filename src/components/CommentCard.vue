<template>
  <div class="card" :reply="reply">
    <div class="card__likes">
      <Likes :likes="likes" @addLikes="addLikes" @removeLikes="removeLikes" />
    </div>
    <div class="card__content">
      <CardHeader
        :avatar="avatar"
        :username="username"
        :date="date"
        :currentUser="currentUser"
        @openModal="openModal"
        @toggleReply="toggleReply"
        @toggleEdit="toggleEdit"
      />
      <CardComment
        :text="text"
        :isEditable="isEditable"
        :modelValue="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        @submit="$emit('submitUpdate', modelValue)"
      />
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
    replies: Array,
    likes: Number,
    avatar: String,
    username: String,
    date: String,
    text: String,
    currentUser: Boolean,
    isEditable: Boolean,
    modelValue: String
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
    },
    openModal() {
      this.$emit('openModal');
    },
    toggleReply() {
      this.$emit('toggleReply');
    },
    toggleEdit() {
      this.$emit('toggleEdit');
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
  position: relative;

  @media (max-width: 550px) {
    flex-direction: column-reverse;
    padding: 10px 14px;
  }
  &:not(:last-child) {
    margin-bottom: 20px;
  }
  &.reply {
    max-width: 600px;
    margin-left: auto;
    @media (max-width: 550px) {
      max-width: 95%;
    }
  }

  &__content {
    width: 100%;
    margin-left: 20px;
    @media (max-width: 550px) {
      margin-left: 0;
    }
  }
}
</style>
