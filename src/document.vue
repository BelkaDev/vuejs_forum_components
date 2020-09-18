<template>
  <v-app >
    <div id="app">
              <createPost v-show="textEditor" ></createPost>

      <v-container>
          <v-layout  row wrap justify-space-between>

          <v-flex d-flex v-if="!expandRight || isClosed" :class="breakPointLeft">
              <div class="left_layout">
              <div class="header_titles">
                <h1 class="header_title">Document Name {{document_id}}</h1>
                <h3 class="header_subtitle">
                  <span v-if="!isPrivate" >All public posts                   <v-icon class="header_icon  ml-1" size="18"
                    >mdi-earth</v-icon> </span>
                   <span v-else >Your private messages  <v-icon class="header_icon  ml-1" size="18"
                    >mdi-account-multiple-outline</v-icon> </span>

                </h3>
              </div>
                    <div class="header_switch ml-2">
                    <v-layout >
                      <v-flex d-flex md8 lg12>
                        <v-icon class="mr-4 mb-1" size="36" style="color:#979797"
                          >mdi-earth</v-icon
                        >
      <v-switch
      v-model="isPrivate"
      inset
    ></v-switch>
                        <v-icon class="" size="36" style="color:#979797"
                          >mdi-account-multiple-outline</v-icon
                        >
                      
                      

              <createPost v-if="!isPrivate"></createPost>
              <createChat v-else></createChat>
     
  
                  </v-flex>
                </v-layout>

                    </div>

                 
<postList v-if="!isPrivate"/>
<discussionList v-else/>
              </div>
          </v-flex>

          <v-flex d-flex :class="breakPointRight">
            <v-layout row wrap class="right_layout pl-0" >
              
<postPage v-show="showPost && !isClosed " :key="id" @layoutPropertiesChanged="updateSize($event)"  :class="expandRight ? 'mt-2' : ''"/>
<chatbox  v-show="showChat && !isClosed " :key="id" @layoutPropertiesChanged="updateSize($event)" :class="expandRight ? 'mt-2' : ''"/>
            </v-layout>
          </v-flex>
          </v-layout>
      </v-container>
    </div>
  </v-app>
</template>

<script>


import postList from "@/components/Forum/postList.vue";
import createPost from "@/components/Forum/createPost.vue";
import postPage from "@/components/Forum/postPage.vue";

import discussionList from "@/components/Chat/discussionList.vue";
import createChat from "@/components/Chat/createChat.vue";
import chatbox from "@/components/Chat/chatbox.vue";

import EventBus from "@/utils/eventBus";


export default {
  data: () => ({
    document_id: 0,
    isPrivate: false,
    expandLeft:true,
    expandRight: false,
    showPost: false,
    textEditor: false,
    showChat: false,
    isClosed: false, // put this in a general layout object
    id: 0,
    breakPointRight: "xs12 sm12 md7 lg7 xl7",
    breakPointLeft: "xs12 sm12 md12 lg12 xl12"
  }),
  components: {
    postList,
    createPost,
    postPage,
    chatbox,
    createChat,
    discussionList,

  },
  methods: {
    updateSize(windowProperties) {
      if (windowProperties.isClosed) {
        this.isClosed = true;
        this.breakPointLeft = "xs12 sm12 md12 lg12 xl12";
      }
      else if (!this.expandRight) {
        this.expandRight = true;
        this.breakPointRight = "xs12 sm12 md12 lg12 xl12 child-flex";
      } else {
        this.expandRight = false;
        this.breakPointRight = "xs12 sm12 md6 lg7 xl6 child-flex";

      }
    },
    openEditor() {
      this.textEditor=true;
    }
  },
    mounted () {
      this.document_id= this.$route.params.id
    // called from postList.vue
    const that = this
    EventBus.$on("openPost", function () {
      that.isClosed=false;
      that.breakPointLeft = "xs12 sm5 md5 lg5 xl5"
      that.expandLeft=false
      that.showPost=true;
      that.showChat=false;
    })
      EventBus.$on("openChat", function () {
      that.isClosed=false;
      that.breakPointLeft = "xs12 sm5 md5 lg5 xl5"
      that.expandLeft=false
      that.showPost=false;
      that.showChat=true;
    })
  },
          watch: {
            '$route' () {
      this.$router.go()
                
            }
        }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: rgba(42, 139, 242, 0.1);
  padding: 0;
  margin: 0;
  max-width: 1000px;
  width: 1000px;
  height: 100%;
}
#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.left_layout {
  width:100%;
  padding-left:1%;
  padding-top: 5%;
}
.header_title {
  color: #444;
  font-size: 30px;
  text-align: left;
}
.header_subtitle {
  color: #666;
  margin-left:3px;
  font-size: 16px;
  text-align: left;
}


 .header_switch {
  float:left;
 }

.header_icon {
  color: #888;
  opacity: 0.80;
}

.right_layout {
  float: right;
  width: 100%;
  margin-top: -2% !important;
  margin-left:2px;
  margin-right: -5px !important;
}
.page_header {
  height: 100px;
  width: 100%;
}
.post_list {
  margin-top: -35%;
}
.chatbox {
  margin: 0px 5px !important;
}
</style>