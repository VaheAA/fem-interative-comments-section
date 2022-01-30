<template>
  <div class="card__header">
    <div class="author">
      <img :src="avatar" alt="" class="avatar" />
      <span class="username">{{ username }}</span>
      <span class="date">{{ date }}</span>
    </div>
    <div class="cta">
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
    date: String
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
    .date {
      margin-left: 15px;
      color: $grayishBlue;
      text-transform: lowercase;
    }
  }
  .cta {
    margin-left: auto;
    .reply-btn {
      background: transparent;
      border: none;
      color: $modBlue;
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 16px;
      cursor: pointer;
    }
  }
}
</style>
