<template>
  <div class="navbar">
    <breadcrumb id="breadcrumb-container" class="breadcrumb-container" v-if="!settingsStore.topNav"/>
    <top-nav id="topmenu-container" class="topmenu-container" v-if="settingsStore.topNav"/>
    <div class="right-menu">
      <template v-if="appStore.device !== 'mobile'">
        <LangSelect class="marTop"></LangSelect>
      </template>
      <div class="avatar-container">
        <el-dropdown @command="handleCommand" class="right-menu-item hover-effect" trigger="click">
          <div class="avatar-wrapper">
            <img src="@/assets/images/profile.jpg" class="user-avatar"/>
            <el-icon>
              <caret-bottom/>
            </el-icon>
          </div>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item command="logout">
                <span>退出登录</span>
              </el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
    </div>
  </div>
</template>

<script setup>
import {ElMessageBox} from 'element-plus'
import Breadcrumb from '@/components/Breadcrumb'
import TopNav from '@/components/TopNav'
import useAppStore from '@/store/modules/app'
import useSettingsStore from '@/store/modules/settings'
import LangSelect from "@/components/LangSelect/index.vue";

const {proxy} = getCurrentInstance();
const appStore = useAppStore()
const settingsStore = useSettingsStore()
const router = useRouter();


function toggleSideBar() {
  appStore.toggleSideBar()
}

function handleCommand(command) {
  switch (command) {
    case "logout":
      logout();
      break;
    default:
      break;
  }
}

function logout() {
  ElMessageBox.confirm(proxy.$t('PublicVariable.Delete_operation_tip'),(proxy.$t('PublicVariable.Popup_title')), {
    confirmButtonText:(proxy.$t('btn.confirm')) ,
    cancelButtonText: (proxy.$t('btn.cancel')),
    type: 'warning'
  }).then(() => {
    router.replace('/login')
    sessionStorage.setItem("isLogin", JSON.stringify({isLogin: false}))
  }).catch(() => {
  });
}

const emits = defineEmits(['setLayout'])

function setLayout() {
  emits('setLayout');
}
</script>

<style lang='scss' scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background 0.3s;
    -webkit-tap-highlight-color: transparent;

    &:hover {
      background: rgba(0, 0, 0, 0.025);
    }
  }

  .breadcrumb-container {
    float: left;
    margin-left: 15px;
  }

  .topmenu-container {
    position: absolute;
    left: 50px;
  }

  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 50px;
    display: flex;

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background 0.3s;

        &:hover {
          background: rgba(0, 0, 0, 0.025);
        }
      }
    }

    .avatar-container {
      margin-right: 40px;

      .avatar-wrapper {
        margin-top: 5px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        i {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }

  .marTop {
    top: 50%;
    margin-top: -5px;
  }
}
</style>
