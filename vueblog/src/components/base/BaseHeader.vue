<template>
  <el-row
    class="blog-header--wrap"
    type="flex"
    justify="flex-wrap"
    align="center"
  >
    <el-col :span="2" :offset="1" :xs="{ span: 24 }" class="blog-logo--wrap">
      <div class="blog-head--logo">
        <el-image
          :src="require('@/assets/img/logo.jpg')"
          fit="cover"
        ></el-image>
      </div>
    </el-col>
    <el-col class="hidden-xs-only" :span="10" :offset="2">
      <el-menu
        class="blog-head--menu"
        mode="horizontal"
        background-color="#2d2f33"
        text-color="#fff"
        active-text-color="#fff"
        default-active="/index"
        ref="navMenu"
        router
      >
        <el-menu-item index="/index">首页</el-menu-item>
        <el-menu-item index="/column">分类</el-menu-item>
        <el-menu-item index="/socket">聊天</el-menu-item>
        <el-menu-item index="/user">个人信息</el-menu-item>
      </el-menu>
    </el-col>
    <el-col :span="4" class="hidden-xs-only">
      <el-input
        class="blog-input--search"
        placeholder="搜索文章"
        v-model="searchVal"
        @keydown.native.enter="search"
      >
        <i
          slot="suffix"
          v-show="searchVal"
          class="el-input__icon el-icon-search"
          @click="search"
        ></i>
      </el-input>
    </el-col>
    <el-col :span="2" :offset="2" class="hidden-xs-only">
      <component :is="loginCompantent"></component>
    </el-col>
  </el-row>
</template>

<script>
import UserImgAvatar from "@/components/user/UserImgAvatar";
import UserLoginHead from "@/components/user/UserLoginHead";
export default {
  name: "BaseHeader",
  components: {
    UserImgAvatar,
    UserLoginHead,
  },
  data (){
    return {
      searchVal:''
    }
  },
  computed: {
    loginCompantent () {
      return this.$store.state.token ? 'UserImgAvatar' : 'UserLoginHead'
    },
  },
  watch: {
    $route (newVal) {
      this.$refs['navMenu'].activeIndex = `/${newVal.name}`
    }
  },
  methods :{
    search () {
      if(this.searchVal){
        if(!this.$route.path.includes('/index')){
          this.$router.push({ name: 'index', query: { searchVal:this.searchVal } })
          this.searchVal = ''
          return
        }
        this.$nextTick(()=>{
          this.$EventBus.$emit('search', this.searchVal)
          this.searchVal = ''
        })
        
      }
    }
/*     search () {
      if(this.searchVal){
        if(!this.$route.path.includes('/index')){
          this.$router.push({ name: 'index', query: { searchVal:this.searchVal } })
          this.searchVal = ''
          return 
        }
        this.$EventBus.$emit('search', this.searchVal)
        this.searchVal = ''       
      }
    } */
  }
};
</script>

<style lang="stylus" >
@import '~assets/css/base.styl';

.blog-header {
  background-color: bg-reverse-color;
}

.blog-header--wrap {
  display: flex;
  align-items: center;
  height: 60px;
}

.blog-head--logo {
  width: 40px;
  height: 40px;
}

.blog-head--logo {
  overflow: hidden;
  border-radius: 50%;
  text-align: center;

  & img {
    display: block;
    width: auto;
    height: 40px;
  }
}

.blog-header--login {
  display: flex;
  align-items: center;
  color: line-reverse-color;
}

.blog-header .blog-input--search {
  max-width: 240px;

  & :focus {
    border-color: transparent;
    box-shadow: 0 0 8px #ccc inset;
  }
}
</style>