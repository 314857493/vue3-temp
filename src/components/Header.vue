<template>
  <div class="Header">
    <!-- <div class="Header-logo"></div> -->
    <img src="@/assets/image/logo.png" height="21" width="140" alt="" />
     <!-- <img src="@/assets/image/shape/logo.svg" height="21" width="140" alt="" /> -->
    <div class="Header-right">
      <el-dropdown @command="handleCommand">
        <span class="el-dropdown-link">
          {{ userName }}<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <template #dropdown>
          <el-dropdown-menu class="dropdown-menu">
            <el-dropdown-item>退出</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
// import Cookies from 'js-cookie';
import pkg from "../../package.json";
import { goLogin } from "../utils";

export default {
  name: "Header",
  props: ["isNeedLogin"],
  data() {
    return {
      pkg,
    };
  },
  computed: {
    userName() {
      return this.$store.state.userInfo.userName || "未登录";
    },
    version() {
      return `v${pkg.version}`;
    },
    needLogin() {
      return process.env.VUE_APP_NO_LOGIN !== "yes" && this.isNeedLogin;
    },
  },
  methods: {
    handleCommand(command) {
      if (command === "out") {
        this.$axios
          .post("/api/cms/system/logout", {
            // params: {
            //   accessToken: Cookies.get('access_token'),
            // },
          })
          .then(() => {
            goLogin();
            // this.$router.push(`/login?returnUrl=${this.$route.fullPath}`);
            // goLogin();
          });
        // const returnUrl = encodeURIComponent(window.location.href);
        // window.location.href = `https://uc.jdcloud.com/redirect/quit?returnUrl=${returnUrl}`;
      }
    },
  },
  mounted() {},
};
</script>

<style lang="scss">
.Header {
  height: 40px;
  line-height: 40px;
  padding: 0 20px;
  background-color: #24292e;
  color: #fff;
  display: flex;
  align-items: center;
  &-logo {
    // background-image: url(~@/assets/image/shape/ai-cms.svg);
    background-image: url(~@/assets/image/logo.png);
    // background-repeat: no-repeat;
    background-size: 140px 21zpx;
    width: 140px;
    height: 21px;
    // cursor: pointer;
  }
  &-right {
    flex: 1;
    text-align: right;
    .el-dropdown {
      color: #fff;
    }
  }
}
</style>
