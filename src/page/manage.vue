<template>
	<div class="manage_page fillcontain">
        <el-row style="height: 8%">
            <head-top></head-top>
        </el-row>
		<el-row class="menuBox" style="height: 92%;">
	  		<el-col :span="4"  style="min-height: 100%; background-color: #545c64;">
				<el-menu  :default-active="defaultActive"  background-color="#545c64"
                          text-color="#fff"
                          active-text-color="#409EFF"
                          router>
					<!--<el-menu-item index="manage"><i class="el-icon-menu"></i>首页</el-menu-item>-->
                    <el-menu-item index="vueEdit"><i class="el-icon-document"></i>文章管理</el-menu-item>
                    <el-menu-item index="video"><i class="el-icon-menu"></i>视频管理</el-menu-item>
                    <el-menu-item index="LabelClassification"><i class="el-icon-star-on"></i>标签/分类管理</el-menu-item>
					<el-submenu index="2">
						<template slot="title"><i class="el-icon-menu"></i>视频管理</template>
						<el-menu-item index="userList">上传管理</el-menu-item>
						<el-menu-item index="shopList">下载管理</el-menu-item>
						<el-menu-item index="foodList">新增视频</el-menu-item>
						<el-menu-item index="orderList">订单列表</el-menu-item>
						<el-menu-item index="adminList">管理员列表</el-menu-item>
					</el-submenu>
					<el-submenu index="3">
						<template slot="title"><i class="el-icon-plus"></i>添加数据</template>
						<el-menu-item index="addShop">添加商铺</el-menu-item>
						<el-menu-item index="addGoods">添加商品</el-menu-item>
					</el-submenu>
					<el-submenu index="4">
						<template slot="title"><i class="el-icon-star-on"></i>图表</template>
						<el-menu-item index="visitor">用户分布</el-menu-item>
						<!-- <el-menu-item index="newMember">用户数据</el-menu-item> -->
					</el-submenu>
					<el-submenu index="5">
						<template slot="title"><i class="el-icon-edit"></i>编辑</template>
						<!-- <el-menu-item index="uploadImg">上传图片</el-menu-item> -->
						<!--<el-menu-item index="vueEdit">文章管理</el-menu-item>-->
					</el-submenu>
					<el-submenu index="6">
						<template slot="title"><i class="el-icon-setting"></i>设置</template>
						<el-menu-item index="adminSet">管理员设置</el-menu-item>
						<!-- <el-menu-item index="sendMessage">发送通知</el-menu-item> -->
					</el-submenu>
					<el-submenu index="7">
						<template slot="title"><i class="el-icon-warning"></i>说明</template>
						<el-menu-item index="explain">说明</el-menu-item>
					</el-submenu>
				</el-menu>
			</el-col>
			<el-col :span="20" style="height: 100%;overflow: auto;">
				<keep-alive>
				    <router-view></router-view>
				</keep-alive>
			</el-col>
		</el-row>
  	</div>
</template>

<script>
    import headTop from '@/components/headTop'
    export default {
        components: {
            headTop,
        },
		computed: {

			defaultActive: function(){
				return this.$route.path.replace('/', '');
			}
		},
    }
</script>


<style lang="less" >
	@import '../style/mixin';
	.manage_page .el-menu-item{
        min-width: 0;
	}
    .menuBox .el-menu-item.is-active {
        color: #ffd04b;
    }
    .menuBox .el-menu{
        background-color: #545c64;
        color: #fff;
        border: none;
    }
    .menuBox .el-submenu__title{
          color: #fff;
    }
    .menuBox .el-menu-item{
        color: #f1f1f1;
    }

</style>

