<template>
  <div class="body">
    <!--header-->
    <div class="header">
      <div class="header-wrap">
        <div class="logo">
          <a href="/">
            <img src="static/imgs/logo.svg" style="width:51px;height:36px;" />
          </a>
        </div>
        <input type="checkbox" name id="mobile-menu-toggle" value />
        <label class="gh" for="mobile-menu-toggle">
          <span></span>
        </label>
        <div class="nav">
          <ul>
            <li>
              <router-link :to="link">{{ link_text }}</router-link>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="hbanner">
      <div class="wrapper">
        <div class="hbanner-txt">
          <h2>
            {{ $t('section_description1_1') }}
            <br />
            <font class="f-blue">{{ $t('section_description1_2') }}</font>
          </h2>
          <div class="btns">
            <a
              href="https://www.showdoc.cc/demo"
              target="_blank"
              class="btn on"
              >{{ $t('demo') }}</a
            >
            <a href="https://www.showdoc.cc/help" target="_blank" class="btn">{{
              $t('more')
            }}</a>
          </div>
        </div>
        <div class="hbanner-imgs"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data() {
    return {
      height: '',
      link: '',
      link_text: '',
      lang: '',
      beian: ''
    }
  },
  methods: {
    getHeight() {
      var winHeight
      if (window.innerHeight) {
        winHeight = window.innerHeight
      } else if (document.body && document.body.clientHeight) {
        winHeight = document.body.clientHeight
      }
      this.height = winHeight + 'px'
    },
    homePageSetting() {
      var url = DocConfig.server + '/api/common/homePageSetting'
      this.axios.post(url, this.form).then(response => {
        if (response.data.error_code === 0) {
          this.beian = response.data.data.beian
          if (response.data.data.home_page == 2) {
            // 跳转到登录页面
            this.$router.replace({
              path: '/user/login'
            })
          }
          if (
            response.data.data.home_page == 3 &&
            response.data.data.home_item
          ) {
            // 跳转到指定项目
            this.$router.replace({
              path: '/' + response.data.data.home_item
            })
          }
        }
      })
    }
  },
  mounted() {
    var that = this
    this.lang = DocConfig.lang
    this.getHeight()
    this.homePageSetting()
    that.link = '/user/login'
    that.link_text = that.$t('login')
    this.get_user_info(function(response) {
      if (response.data.error_code === 0) {
        that.link = '/item/index'
        that.link_text = that.$t('my_item')
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped src="@/../static/css/qietu.css"></style>
<style scoped src="@/../static/css/style.css"></style>
<style scoped src="@/../static/css/responsive.css"></style>
