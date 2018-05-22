<template>
    <div class="header_container">
        <div style="font-size: 20px;text-align: left">播斗运营管理后台</div>
		<!--<el-breadcrumb style="color: #fff" separator="/">-->
			<!--<el-breadcrumb-item :to="{ path: '/vueEdit' }" >文章管理</el-breadcrumb-item>-->
			<!--<el-breadcrumb-item v-for="(item, index) in $route.meta" key="index">{{item}}</el-breadcrumb-item>-->
		<!--</el-breadcrumb>-->
        <!--<div style="width: 20%"></div>-->
        <!--<div style="width: 20%"></div>-->
		<el-dropdown @command="handleCommand" menu-align='start'>
			<img :src="baseImgPath + adminInfo.avatar" class="avator">
			<el-dropdown-menu slot="dropdown">
				<el-dropdown-item command="singout">退出</el-dropdown-item>
			</el-dropdown-menu>
		</el-dropdown>
    </div>
</template>

<script>
	import {signout} from '@/api/getData'
	import {baseImgPath} from '@/config/env'
	import {mapActions, mapState} from 'vuex'

    export default {
    	data(){
    		return {
    			baseImgPath,
    		}
    	},
    	created(){
    		if (!this.adminInfo.id) {
    			this.getAdminData()
    		}
    	},
    	computed: {
    		...mapState(['adminInfo']),
    	},
		methods: {
			...mapActions(['getAdminData']),
			async handleCommand(command) {
				if (command == 'home') {
					this.$router.push('/vueEdit');
				}else if(command == 'singout'){
					const res = await signout()
					if (res.status == 1) {
						this.$message({
	                        type: 'success',
	                        message: '退出成功'
	                    });
	                    this.$router.push('/');
					}else{
						this.$message({
	                        type: 'error',
	                        message: res.message
	                    });
					}
				}
			},
		}
    }
</script>

<style lang="less">
	@import '../style/mixin';
	.header_container{
		background-color: #545c64;
		height: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-left: 20px;
        border-bottom: 1px solid #ddd;
        color: #fff;
	}
	.avator{
		.wh(36px, 36px);
		border-radius: 50%;
		margin-right: 37px;
	}
	.el-dropdown-menu__item{
        text-align: center;
    }
    .header_container .el-breadcrumb__item__inner{
        color: #f0f0f0!important;
    }
</style>
