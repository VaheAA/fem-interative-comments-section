<template>
  <div class="card__header" :currentUser="currentUser">
    <div class="author">
      <img :src="avatar" alt="" class="avatar" />
      <span class="username">{{ username }}</span>
      <span v-if="currentUser" class="current">you</span>
      <span class="date">{{ date }}</span>
    </div>
    <div class="cta">
      <button class="delete-btn" v-if="currentUser" @click="deleteComment">
        <img src="../assets/images/icon-delete.svg" alt="" />
        Delete
      </button>
      <button class="reply-btn">
        <img src="../assets/images/icon-reply.svg" />
        Reply
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardHeader',
  props: {
    avatar: String,
    username: String,
    date: String,
    currentUser: Boolean
  },
  methods: {
    deleteComment() {
      this.$emit('deleteComment');
    }
  }
};
</script>

<style lang="scss">
@use '../assets/scss/style.scss' as *;
.card__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin-bottom: 25px;

  .author {
    display: flex;
    align-items: center;
    font-size: 14px;

    .avatar {
      width: 30px;
    }
    .username {
      margin-left: 15px;
      text-transform: lowercase;
      font-weight: 500;
    }
    .current {
      margin-left: 15px;
      background-color: $modBlue;
      color: $white;
      padding: 3px 5px;
      border-radius: 3px;
      font-size: 12px;
    }
    .date {
      margin-left: 15px;
      color: $grayishBlue;
      text-transform: lowercase;
    }
  }
  .cta {
    margin-left: auto;
    display: inline-flex;
    .reply-btn {
      background: transparent;
      border: none;
      color: $modBlue;
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 14px;
      cursor: pointer;
      transition: all 0.2s ease;

      &:hover {
        opacity: 0.5;
      }
    }
    .delete-btn {
      background: transparent;
      border: none;
      display: flex;
      align-items: center;
      color: $softRed;
      gap: 10px;
      font-size: 14px;
      cursor: pointer;
      margin-right: 5px;
      transition: all 0.2s ease;

      &:hover {
        opacity: 0.5;
      }
    }
  }
}
</style>
