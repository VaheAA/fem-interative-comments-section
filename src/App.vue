<template>
  <div class="container">
    <CommentCard
      v-for="comment in comments"
      :key="comment.id"
      :likes="comment.score"
      :avatar="comment.user.image.png"
      :username="comment.user.username"
      :date="comment.createdAt"
      :text="comment.content"
      @addLikes="increaseLikes(comment.id)"
      @removeLikes="decreaseLikes(comment.id)"
    />
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
      this.comments.forEach((comment, idx) => {
        if (id - 1 === idx) {
          comment.score += 1;
          return localStorage.setItem(
            'comments',
            JSON.stringify(this.comments)
          );
        }
      });
    },
    decreaseLikes(id) {
      this.comments.forEach((comment, idx) => {
        if (id - 1 === idx) {
          return (comment.score -= 1);
        }
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
</style>
