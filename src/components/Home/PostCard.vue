<template lang="html">
  <div class="card p-0" @click="loadSinglePost(post.id)" itemscope itemprop="url"> <!-- itemscope : search engine -->
    <div class="no_picture" v-if="!post.imageURLs">
      <i class="material-icons">photo</i>
    </div>
    <div class="imageURLs" v-else>
      <img class="__image" :src="selectedImage" alt="Card image cap" v-if="post.imageURLs" style="background: #bdc3c7;" itemprop="image">
      <div class="imageURLsSize"><i class="material-icons mr-1">camera_alt</i>{{imageURLsSize}}</div>
    </div>
    <div class="card-body">
      <h5 class="card-title mb-0 post__title" itemprop="name">{{postTitle}}</h5>
      <p class="mb-0"><small class="card-subtitle text-muted" itemprop="date">Posté: {{post.timestamp | date}}</small></p>
      <p class="__author mb-1">
        <small class="card-subtitle __text" itemprop="author">Par: {{post.creatorName}}</small>
      </p>
      <p class="card-text" itemprop="description">{{post.description}}</p>
      <div class="d-flex justify-content-between align-items-end align-text-bottom mt-3">
        <a class="card-link __price" itemprop="price">{{post.price}}{{post.currency | currency}}</a>
        <router-link tag="a" :to="`/posts/${post.id}`" class="btn btn__more" itemprop="url" style="font-size: 14px;">VOIR</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['post'],
  computed: {
    selectedImage () {
      return Object.values(this.post.imageURLs)[0]
    },
    postTitle () {
      return _.capitalize(this.post.title);
    },
    postInWish () {
      return this.user.registeredWish.findIndex(postId => postId === this.post.id) >= 0
    },
    imageURLsSize () {
      return _.size(this.post.imageURLs)
    }
  },
  methods: {
    loadSinglePost (id) {
      return this.$router.push(`/posts/${id}`)
    },
    triggerWish (id) {
       if (this.postInWish) {
         this.$store.dispatch('unsaveWish', id)
       }else{
         this.$store.dispatch('saveWish', id)
       }
     }
  }
}
</script>

<style lang="scss" scoped>
$app-color: #3498db;
small {
  font-weight: lighter;
}
.imageURLs {
  position: relative;
  background: #FFF;
  display: flex;
  justify-content: center;
  border-bottom: .5px solid #eee;
  img {
    width: 100%;
  }
  .imageURLsSize {
    position: absolute;
    top: 10px;
    left: 10px;
    display: flex;
    flex-direction: row;
    align-items: center;
    background: rgba(0,0,0,.6);
    padding: 2px 6px;
    border-radius: 5px;
    font-size: 14px;
    color: #FFF;
    i {
      font-size: 18px;
    }
  }
}
.__author {
  margin-top: -5px;
}
.no_picture {
  background: #000;
  background: rgba(0,0,0,.85);
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  height: 200px;
  i {
    font-size: 62px;
    color: #FFF;
  }
}
.__image{
  height: 180px;
  background: #FFF;
  display: flex;
  justify-content: center;
}
a:hover{
  color: #FFF;
}
.card{
  transition: .1s ease-in;
  box-shadow: 0 10px 40px 0 rgba(62, 57, 107, 0.06), 0 2px 9px 0 rgba(62, 57, 107, 0.05);
  &:hover{
    box-shadow: none;
    cursor: pointer;
  }
}
.__price {
  font-size: 22px;
  color: #039be5;
  font-weight: 500;
}
@media only screen and (max-width: 767px) {
  .__price {
    font-size: 20px;
  }
}
</style>
