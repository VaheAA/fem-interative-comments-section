<template>
  <div class="container">
    <template v-for="comment in comments" :key="comment.id">
      <CommentCard
        :likes="comment.score"
        :avatar="comment.user.image.png"
        :username="comment.user.username"
        :date="comment.createdAt"
        :text="comment.content"
        @addLikes="increaseLikes(comment.id)"
        @removeLikes="decreaseLikes(comment.id)"
      />
      <div class="replies">
        <CommentCard
          v-for="reply in comment.replies"
          :key="reply.id"
          :likes="reply.score"
          :avatar="reply.user.image.png"
          :username="reply.user.username"
          :date="reply.createdAt"
          :text="reply.content"
          @addLikes="increaseLikes(reply.id)"
          @removeLikes="decreaseLikes(reply.id)"
          :reply="comment.replies ? true : false"
          :class="{reply: 'reply'}"
        />
      </div>
    </template>
  </div>
</template>

<script>
import CommentCard from './components/CommentCard.vue';
import data from '../public/data.json';

export default {
  name: 'App',
  components: {CommentCard},
  data() {
    return {
      comments: data.comments
    };
  },
  created() {
    this.comments = JSON.parse(localStorage.getItem('comments'));
  },
  methods: {
    increaseLikes(id) {
      this.comments.forEach((comment) => {
        if (id === comment.id) {
          comment.score += 1;
          return localStorage.setItem(
            'comments',
            JSON.stringify(this.comments)
          );
        }
      });
      this.comments.forEach((comment) => {
        comment.replies.forEach((reply) => {
          if (id === reply.id) {
            reply.score += 1;
            return localStorage.setItem(
              'comments',
              JSON.stringify(this.comments)
            );
          }
        });
      });
    },
    decreaseLikes(id) {
      this.comments.forEach((comment) => {
        if (id === comment.id) {
          comment.score -= 1;
          return localStorage.setItem(
            'comments',
            JSON.stringify(this.comments)
          );
        }
      });
      this.comments.forEach((comment) => {
        comment.replies.forEach((reply) => {
          if (id === reply.id) {
            reply.score -= 1;
            return localStorage.setItem(
              'comments',
              JSON.stringify(this.comments)
            );
          }
        });
      });
    }
  }
};
</script>

<style lang="scss">
@use './assets/scss/style.scss' as *;

html {
  box-sizing: border-box;
  font-size: 16px;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body,
h1,
h2,
h3,
h4,
h5,
h6,
p,
ol,
ul {
  margin: 0;
  padding: 0;
  font-weight: normal;
}

ol,
ul {
  list-style: none;
}

img {
  max-width: 100%;
  height: auto;
}
html {
  height: 100%;
}
body {
  background-color: $lightGray;
  font-family: 'Rubik', sans-serif;
  height: 100%;
}
.container {
  max-width: 750px;
  margin: 0 auto;
  padding: 50px 20px;
}
.replies {
  position: relative;

  &::after {
    content: '';
    position: absolute;
    width: 2px;
    height: 100%;
    background: $lightGrayishBlue;
    left: 45px;
    top: 0;
  }
}
</style>
