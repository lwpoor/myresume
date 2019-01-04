<template>
  <div class="Index">
    <aside class="sidebar">
      <div class="avatar">
        <img src="http://img.lwpoor.cn/resume/avatar.jpg" title="王袁龙"/>
      </div>
      <nav class="nav">
        <a v-if="posts.userinfo" href="#info">资料</a>
        <a v-if="posts.skill != []" href="#skills">技能</a>
        <a v-if="posts.perwork != []" href="#works">作品</a>
        <a v-if="posts.work != []" href="#story">经历</a>
        <a v-if="posts.project != []" href="#team">项目</a>
      </nav>
    </aside>
    <main>
      <section id="info">
        <div class="wrap">
          <h2 class="title">个人资料</h2>
          <div class="row">
            <div class="col-l-4">
              <p>姓名：{{posts.userinfo.name}}</p>
              <p>性别：{{posts.userinfo.sex}}</p>
              <p>年龄：{{posts.userinfo.age}}</p>
              <p>英文名：{{posts.userinfo.englishName}}</p>
              <p>学历：{{posts.userinfo.education}}</p>
            </div>
            <div class="col-l-4">
              <p>联系电话：{{posts.userinfo.phone}}</p>
              <p>GitHub：<a :href="posts.userinfo.github">{{posts.userinfo.github}}</a></p>
              <p>博客：<a :href="posts.userinfo.blog">{{posts.userinfo.blog}}</a></p>
              <p>邮箱：<a>{{posts.userinfo.email}}</a></p>
            </div>
            <div class="col-l-4">
              <p v-html="posts.userinfo.remarks"></p>
            </div>
          </div>
        </div>
      </section>
      <section v-if="posts.skill != []" id="skills">
        <div class="wrap">
          <h2 class="title">掌握的技能</h2>
          <div id="skillsList" class="row">
            <div v-for="item in posts.skill" class="col-s-6 col-m-4 center-fixed">
              <div class="skills-icon">
                <i :class="item.img"></i>
              </div>
              <div class="skills-title">
                <h3>{{item.name}}</h3>
                <p>{{item.des}}</p>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section v-if="posts.perwork != []" id="works">
        <div class="wrap">
          <h2 class="title">个人作品</h2>
          <div class="row">
            <div v-for="item in posts.perwork" class="col-s-6">
              <div class="works-item">
                <img :src='item.img'/>
                <p>{{item.name}}</p>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section v-if="posts.work != []" id="story">
        <div class="wrap">
          <h2 class="title">个人经历</h2>
          <div class="row">
            <div class="col-m-8">
              <ul class="timeline">
                <li v-for="item in posts.work">{{item.startDate|formatDate}}至{{item.endDate | formatDate}}：{{item.name}}：<a href="" target="_blank"><span v-html="item.des1"></span></a> <span v-html="item.des2"></span></li>
              </ul>
            </div>
            <!--<div class="col-m-4 center-fixed">-->
              <!--<img src="http://img.lwpoor.cn/resume/story.png"/>-->
            <!--</div>-->
          </div>
        </div>
      </section>
      <section v-if="posts.project != []" class="content-d" id="team">
        <br class="wrap">
          <h2 class="title">项目经历</h2>
          <div v-for="item in posts.project" class="row">
            <div class="col-m-6">
              <h3>{{item.name}}</h3>
              <p v-html="item.des"></p>
              <p v-html="item.zeren"> </p>
              <p v-html="item.technology"></p>
              <p>项目地址：<a :href="item.link" target="_blank">{{item.link}}</a></p>
            </div>
            <div class="col-m-6 center">
              <img :src="item.img" style="height:400px;"/>
            </div>
          </div>
      </section>
      <footer>
        <p>© 2019 By <a href="http://www.lwpoor.cn" title="lwpoor" target="_blank">lwpoor</a>.</p>
      </footer>
    </main>

  </div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      posts: {
        userinfo:{},
        perwork:[],
        work:[],
        skill:[],
        project:[]
      }
    }
  },
  methods:{
    getData(){
      this.$http({
        url:'http://m.lwpoor.cn/resume/get',
        method:'get',
        params:{
          phone: getUrlKey("phone")
        }
      })
        .then((response) => {
          if (response.data != null){
            this.posts = response.data;
          }
        })
        .catch(function (error) {
          console.log(error);
        })
    }
  },
  beforeMount(){
    this.getData();
  },
  filters: {
    formatDate: function (str) {
      if (!str) return '';
      var date=str.replace("/Date(","");
      date=date.replace(")/","");
      var newDate = new Date(parseInt(date));
      var Y = newDate.getFullYear();
      var M = (newDate.getMonth()+1 < 10 ? '0'+(newDate.getMonth()+1) : newDate.getMonth()+1);
      var D = newDate.getDate();
      var h = newDate.getHours();
      var m = newDate.getMinutes();
      var s = newDate.getSeconds();

      return Y+"年"+M+"月"+D+"日";
    }
  }
}

function getUrlKey(name) {
  return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.href) || [, ""])[1].replace(/\+/g, '%20')) || null;
}

import kico from '../assets/kico.js'
kico.bk_image("main img");
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
