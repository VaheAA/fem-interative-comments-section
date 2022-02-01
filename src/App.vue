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
        @toggleReply="toggleReply(comment)"
        @toggleEdit="toggleEdit(comment)"
        @submitUpdate="submitUpdate"
        :currentUser="
          currentUser.username === comment.user.username ? true : false
        "
        :isEditable="isEditable && commentToEdit.id === comment.id"
        v-model="editContent"
      />
      <div class="replies" v-for="reply in comment.replies" :key="reply.id">
        <CommentCard
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
          @toggleEdit="toggleEdit(reply)"
          :currentUser="
            currentUser.username === reply.user.username ? true : false
          "
          :isEditable="isEditable && commentToEdit.id === reply.id"
          v-model="editContent"
          @submitUpdate="submitUpdate"
        />
      </div>
      <AddReply
        v-if="isActive && commentForReply.id === comment.id"
        :avatar="currentUser.image.png"
        :username="currentUser.username"
        v-model="replyText"
        @submitReply="submitReply"
      />
    </template>
    <AddComment
      :avatar="currentUser.image.png"
      :username="currentUser.username"
      v-model="commentText"
      @submitForm="submitForm"
    />
    <Modal
      :isOpen="isOpen"
      @closeModal="closeModal"
      @deleteComment="deleteComment(commentToDelete)"
    />
  </div>
</template>

<script>
import CommentCard from './components/CommentCard.vue';
import data from '../public/data.json';
import AddComment from './components/AddComment.vue';
import Modal from './components/Modal.vue';
import AddReply from './components/AddReply.vue';

export default {
  name: 'App',
  components: {CommentCard, AddComment, Modal, AddReply},
  data() {
    return {
      comments: data.comments,
      currentUser: data.currentUser,
      commentText: '',
      replyText: '',
      isOpen: false,
      commentToDelete: null,
      commentForReply: null,
      commentToEdit: null,
      editContent: '',
      isActive: false,
      isEditable: false,
      timestamp: Date.now()
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
      this.commentToDelete = id;
      document.body.style.overflow = 'hidden';
    },
    toggleReply(item) {
      this.isActive = true;
      this.commentForReply = item;
    },
    toggleEdit(item) {
      this.isEditable = true;
      this.commentToEdit = item;
      this.editContent = this.commentToEdit.content;
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
      this.commentToDelete = null;
      document.body.style.overflow = 'scroll';
    },
    closeModal() {
      this.isOpen = false;
      document.body.style.overflow = 'scroll';
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
        createdAt: this.createTimeStamp(),
        score: 0,
        replies: []
      };
      if (this.commentText) {
        this.comments.push(newComment);
        this.commentText = '';
        return localStorage.setItem('comments', JSON.stringify(this.comments));
      } else {
        alert('Please enter something befor submitting');
      }
    },
    submitReply() {
      const newReply = {
        id: Math.floor(Math.random() * 100000000000),
        content: this.replyText,
        createdAt: this.createTimeStamp(),
        score: 0,
        replyingTo: this.commentForReply.user.username,
        user: {
          image: {
            png: 'https://i.imgur.com/WfguSm8.png'
          },
          username: 'juliusomo'
        }
      };
      if (this.replyText) {
        this.commentForReply.replies.push(newReply);
        this.replyText = '';
        this.isActive = false;
        return localStorage.setItem('comments', JSON.stringify(this.comments));
      } else {
        alert('Please enter something befor submitting');
      }
    },
    submitUpdate() {
      if (this.editContent) {
        this.commentToEdit.content = this.editContent;
        this.isEditable = false;
        return localStorage.setItem('comments', JSON.stringify(this.comments));
      } else {
        alert('Please enter something befor submitting');
      }
    },
    createTimeStamp() {
      const day = new Date(this.timestamp);
      const stringifiedDate = day.toString();
      const formatedDate = stringifiedDate.slice(4, 24);
      return formatedDate;
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
    left: 0;
    top: 0;
  }
}
</style>
