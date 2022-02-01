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
        @openModal="openModal(comment.id)"
        @toggleReply="toggleReply"
        :currentUser="
          currentUser.username === comment.user.username ? true : false
        "
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
          @openModal="openModal(reply.id)"
          @toggleReply="toggleReply"
          :currentUser="
            currentUser.username === reply.user.username ? true : false
          "
        />
      </div>
    </template>
    <AddComment
      :avatar="currentUser.image.png"
      :username="currentUser.username"
      v-model="commentText"
      :isReply="isReply"
      @submitForm="submitForm"
    />
    <Modal
      :isOpen="isOpen"
      @closeModal="closeModal"
      @deleteComment="deleteComment(currentComment)"
    />
  </div>
</template>

<script>
import CommentCard from './components/CommentCard.vue';
import data from '../public/data.json';
import AddComment from './components/AddComment.vue';
import Modal from './components/Modal.vue';

export default {
  name: 'App',
  components: {CommentCard, AddComment, Modal},
  data() {
    return {
      comments: data.comments,
      currentUser: data.currentUser,
      commentText: '',
      isOpen: false,
      currentComment: null,
      selectedComment: null,
      isReply: false
    };
  },
  created() {
    if (localStorage.getItem('comments')) {
      this.comments = JSON.parse(localStorage.getItem('comments'));
    }
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
    },
    openModal(id) {
      this.isOpen = true;
      this.currentComment = id;
    },
    toggleForm() {
      this.isReply = true;
    },
    deleteComment(id) {
      this.comments = this.comments.filter((comment) => {
        return comment.id !== id;
      });
      this.comments.forEach((comment) => {
        comment.replies = comment.replies.filter((reply) => reply.id !== id);
        return localStorage.setItem('comments', JSON.stringify(this.comments));
      });
      this.isOpen = false;
      this.currentComment = null;
    },
    closeModal() {
      this.isOpen = false;
    },
    submitForm() {
      const newComment = {
        id: Math.floor(Math.random() * 100000000000),
        content: this.commentText,
        user: {
          image: {
            png: 'https://i.imgur.com/WfguSm8.png'
          },
          username: 'juliusomo'
        },
        createdAt: '1 week ago',
        score: 0,
        replies: []
      };
      const newreply = {
        id: Math.floor(Math.random() * 100000000000),
        content: this.commentText,
        createdAt: '1 week ago',
        score: 0,
        replyingTo: this.selectedComment.user.username,
        user: {
          image: {
            png: 'https://i.imgur.com/WfguSm8.png'
          },
          username: 'juliusomo'
        }
      };
      if (this.commentText) {
        this.comments.push(newComment);
        this.commentText = '';
        return localStorage.setItem('comments', JSON.stringify(this.comments));
      } else {
        alert('Please enter something befor submitting');
      }
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
textarea::placeholder {
  font-family: inherit;
  font-size: 14px;
  color: inherit;
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
  margin-bottom: 20px;

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
