<template>
  <div id="app">
    <div class="container">
      <div class="row mt-5 mb-4">
        <div class="col-12">
          <b-form-select v-model="selectedUser" :options="usersWithDefault" :disabled="isUserSelectDisabled"></b-form-select>
        </div>
      </div>
      <div class="row">
        <Post
          v-for="post in postsFilteredByUser"
          :key="post.id"
          :id="post.id"
          :userId="post.userId"
          :userName="getUserNameById(post.userId)"
          :title="post.title"
          :body="post.body"
        ></Post>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Post from "./components/Post.vue";

export default {
  name: "App",
  components: {
    Post
  },

  data(){
    return{
        apiBaseUrl:'http://jsonplaceholder.typicode.com/',
        usersApi:'users',
        postsApi:'posts',
        selectedUser: null,
        users: [],
        posts:[]
    }
  },

  computed:{
    /**
     * Список пользователей, дополненный нулевым значением
     */
    usersWithDefault(){
      return [
        { value: null, text: 'All'},
        ...this.users
      ]
    },

    /**
     * Определяет кликабельность списка пользователей, в зависимости от того подгружен ли список
     */
    isUserSelectDisabled(){
      return this.users.length>0?false:true;
    },

    /**
     * Отфильтрованные посты по автору
     */
    postsFilteredByUser(){
      if (this.selectedUser===null){
        return this.posts
      }
      else{
        return this.posts.filter((post)=>{
          return post.userId===this.selectedUser;
        })
      }
    }
  },

  methods:{

    /**
     * Отправление запроса на получение списка пользователей
     */
    async getUsers(){
      try{
        const response = await axios.get(`${this.apiBaseUrl}${this.usersApi}`);
        const users = response.data;
        this.users = users.map((user)=>{
          return {
            value:user.id,
            text:user.name
          }
        });
      } catch (error){
        console.log(error);
      }
    },

    /**
     * Отправление запроса на получение списка постов
     */
    async getPosts(){
      try{
        const response = await axios.get(`${this.apiBaseUrl}${this.postsApi}`);
        const posts = response.data;
        console.log(posts);
        this.posts = posts;
      } catch (error){
        console.log(error);
      }
    },

    getUserNameById(id){
      console.log(id);
      return this.users.filter((user)=>{
        return user.value==id;
      })
      .pop().text;
    }

  },

  mounted(){
    this.getUsers();
    this.getPosts();
  }
};
</script>

<style lang="scss">
  @import 'node_modules/bootstrap/scss/bootstrap.scss';
  @import "~@/assets/scss/vendors/bootstrap-vue/index";
</style>
