<template>
  <div style="display: flex; height: 100vh;">

    <div>
      <el-col :span="12" style="width: 25vh; min-width: 200px">
        <el-menu style="height: 100vh;"


                 class="el-menu-vertical-demo"
                 @select="handleMenuSelection"

                 background-color="#001529"
                 text-color="#fff"
                 active-text-color="#ffd04b">
          <div style="text-align: center; color: #ffffff; font-size: 20px;">
            <!-- <img src="../assets/logo.jpg" style="height: 18px;"/> -->
            南京鼓楼医院集团管理系统
          </div>

          <el-submenu index="1" >
            <template slot="title">
              <i class="el-icon-document"></i>
              <span>入出库管理</span>
            </template>
            <el-menu-item-group >
              <el-menu-item index="1-1">货品基本信息维护</el-menu-item>
              <el-menu-item index="1-2">货品入库</el-menu-item>
              <el-menu-item index="1-3">货品出库</el-menu-item>
              <el-menu-item index="1-4">入出库结算管理</el-menu-item>
              <el-menu-item index="1-5">制造商信息管理</el-menu-item>
              <el-menu-item index="1-6">供应商信息管理</el-menu-item>
              <el-menu-item index="1-7">报表管理</el-menu-item>


            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2" >
            <template slot="title">
              <i class="el-icon-document"></i>
              <span>销售管理</span>
            </template>
            <el-menu-item-group >
              <el-menu-item index="2-1">客户基本信息维护</el-menu-item>
              <el-menu-item index="2-2">购买记录管理</el-menu-item>


            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-col>
    </div>
    <!-- Sidebar -->


    <!-- Main Content -->
    <div style="flex-grow: 1; display: flex; flex-direction: column;">
      <!-- Header -->
      <div style="text-align: right; padding: 10px; background-color: #ffffff;border-bottom: 1px solid #ccc;">

        <div style="" >
          <el-dropdown @command="userHandleCommand">
        <span style="font-size: 18px">

           {{ username }}
          <i class="el-icon-arrow-down el-icon--right"></i>
        </span>

            <el-dropdown-menu>
              <el-dropdown-item command="personalSettings">个人设置</el-dropdown-item>
              <el-dropdown-item command="logout">退出登录</el-dropdown-item>
            </el-dropdown-menu>

          </el-dropdown>
        </div>
      </div>
      <!-- Content -->
      <div style="flex-grow: 1; ">
        <router-view></router-view>
      </div>
    </div>

  </div>
</template>


<script>





import service from "@/main";

export default {
  name: "MainPage",
  data() {
    return {
      status:"注册",


      username: localStorage.getItem("username"),
      dialogFormVisible: false,
      passwordForm: {
        oldPassword:"",
        newPassword:"",
        confirmPassword:"",

      },
      respondNum:-1,
      formLabelWidth: '120px',
      updateLog:[],

    }


  },
  watch: {
    $route: {
      //初始化就执行一次
      handler() {

        this.user= localStorage.getItem("user")
        if(localStorage.getItem("user")===null){
          this.dialogFormVisible=false
        }

      },
      immediate: true,
    }
  },
  mounted() {
    this.queryManufacturerInformation();
  },


  methods: {
    queryManufacturerInformation() {
      // Create a Promise for each service.get call
      const fetchManufacturerList = service.get('/queryManufacturerList', {
        params: {
          currentPage: 1,
          pageSize: 2**31 - 1,
        }
      }).then(response => {
        console.log(response);
        this.manufacturerList = response.data.data;
      }).catch(error => {
        console.error(error);
      });

      const fetchManufacturerListCount = service.get('/queryManufacturerListCount', {
        params: {
          currentPage: 1,
          pageSize: 2**31 - 1,
        }
      }).then(response => {
        console.log(response);
        this.manufacturerListCount = response.data.data;
      }).catch(error => {
        console.error(error);
      });

      // Return a Promise that resolves when both requests are completed
      return Promise.all([fetchManufacturerList, fetchManufacturerListCount]);
    },
    handleMenuSelection(index){
      if(index==="1-1"){
        this.$router.push({name: 'ItemInformationMaintenance'})
      }
      else if(index==="1-2"){
        this.$router.push({name: 'InboundManagement'})
      }
      else if(index==="1-3"){
        this.$router.push({name: 'OutboundManagement'})
      }
      else if(index==="1-4"){
        this.$router.push({name: 'CheckOutManagement'})
      }
      else if(index==="1-5"){
        this.$router.push({name: 'ManufacturerInformationMaintenance'})
      }
      else if(index==="1-6"){
        this.$router.push({name: 'SupplierInformationMaintenance'})
      }
      else if(index==="1-7"){
        this.$router.push({name: 'StatementManagement'})
      }


      else if(index==="2-1"){
        this.$router.push({name: 'CustomerInformationMaintenance'})
      }
      else if(index==="2-2"){
        this.$router.push({name: 'PurchaseRecordManagement'})
      }
    },





    userHandleCommand(command) {
      if(command==="logout"){
        localStorage.removeItem("username")
        localStorage.removeItem("token")
        this.$router.replace({name:"login"})
      }
      else if(command==="changePassword"){
        this.dialogFormVisible = true
      }
      else if(command==="staffManagement"){
        this.$router.push('/StaffManagement')
      }
      else if(command==="personalSettings"){
        this.$router.push('/PersonalSettings')
      }
      else if(command==="personalInformation"){
        this.$router.push('/PersonalInformation')
      }else if(command==="help"){
        this.$router.push('/help')

      }else if(command==="statements"){
        this.$router.push('/Statements')

      }

    },
    cancelUpdate(){
      this.passwordForm={
        oldPassword:"",
        newPassword:"",
        confirmPassword:"",
      }
      this.dialogFormVisible = false
    },




  }

}


</script>

<style scoped>
.el-menu-item {
  width: 100%; /* Ensures all menu items fill the container but do not exceed it */
  max-width: 240px; /* Adjust this value based on your design needs */
}
</style>
