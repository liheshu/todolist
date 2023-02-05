<template>
	<view class="content">
		<view class="todoListBox">
			<view class="header-box">
				<view class="header-left">
					<view class="add">+</view>
						<h2>Todo List</h2>
					</view>
				<view class="btns">
					<button class="warning" @click="handSelectAll">全选</button>
					<button class="msg" @click="handleAdd()">添加</button>
				</view>
			</view>
			<view class="goods" v-for="(item,index) in todoList" :key="item.id" :class="item.isCheck?'selectClass':''">
			<view class="goodsleft">
				<view class="check" @click="handSelect(index,item.id)">
					<view :class="item.isCheck?'checked':''">
							
						</view>
					</view>
				</view>
				
				<!-- input输入 -->
				<input type="text" class="content-input"  placeholder="请输入" :disabled="item.isCheck" :class="item.isCheck?'line-through':''" @blur='handleBlur' v-model="item.text" ref="inputBox">
				<view class="info">
					
				</view>
				<!-- info -->
					<!-- <view class="time">2022-10-20</view> -->
					<button class="warning del" @click="delGood(index,item.id)">删除</button>
			</view>

			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				allSelect:false,
				todoList:[]
			}
		},
		created() {
			let getList=JSON.parse(uni.getStorageSync('listTodo'))
			if(getList===null){
				this.todoList=[{
				id:this.randomID(),
				text:'请点击上方添加按钮添加事件',
				isCheck:false,
				}]
			}else{
				this.todoList=getList
			}
				
		},
		methods: {
		handleAdd(){
				// alert('666')
			this.todoList.unshift({
				id:this.randomID(),
				isCheck:false, //是否选中
				text:'' ,//内容
					})
				},
				// 生成随机id
			randomID(){
				return Math.random().toString(16).slice(2);
			},
			delGood(index,id){
				// console.log(index,id);
				if(this.todoList[index].id===id){
					this.todoList.splice(index,1)
				}
				this.storage()
				
			},
			// 选中
			handSelect(index,id){
				// console.log('666');
				if(this.todoList[index].id===id){
					this.todoList[index].isCheck=!this.todoList[index].isCheck
					}
					this.storage()
			},
			// 全选
			handSelectAll(){
				// 遍历  把所有ischeck变为return true;
				this.allSelect=!this.allSelect
				this.todoList.forEach(item=>{
					item.isCheck=this.allSelect
				})
				this.storage()
			},
			// 判断是否输入完成
			handleBlur(){
				this.storage()
				// console.log('666');
			},
			// 本地存储
			storage(){
				uni.setStorageSync('listTodo',JSON.stringify(this.todoList))
			}
		}		
	}
</script>

<style lang="scss">
	.todoListBox{
		margin-top: 40rpx;
		padding: 40rpx;
		// height: 900rpx;
		border-radius: 40rpx;
		// background: pink;
	}
	.warning {
		background-color: #ce2a29;
	}
	.msg{
		background-color: #49b05d;
	}
	.header-box{
		height: 150rpx;
		// background-color: blue;
		border-bottom:4rpx solid #ccc;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: space-between;
		.header-left{
			display: flex;
			.add{
				width: 80rpx;
				height: 80rpx;
				background-color: #7877c4;
				border-radius: 40rpx;
				margin-top: 35rpx;
				font-size: 72rpx;
				color: white;
				line-height: 70rpx;
				text-align: center;
			}
			h2{
				line-height: 150rpx;
				color: #ccc;
				margin-left: 20rpx;
			}
		}
		.btns{
			display: flex;
			button{
				width: 140rpx;
				height: 80rpx;
				margin-top: 35rpx;
				margin-right: 20rpx;
				font-size: 36rpx;
				text-align: center;
				line-height: 81rpx;
				color: white;
				font-weight: 700;
			}
		}
	}
	.goods{
		margin-top: 40rpx;
		height: 160rpx;
		border-radius: 20rpx;
		background-color: #ee8980;
		display: flex;
		justify-content: space-between;
		.check{
			width: 60rpx;
			height: 60rpx;
			// background-color: greenyellow;
			margin-top: 40rpx;
			border-radius: 40rpx;
			border: 4rpx solid #ccc;
			margin-left: 10rpx;
			position: relative;
			.checked{
				width: 40rpx;
				height: 40rpx;
				background-color: #908dde;
				border-radius: 20rpx;
				position: absolute;
				top: 50%;
				left: 50%;
				transform: translate(-50%,-50%);
				
			}
		}
	}
	.content-input{
		width: 400rpx;
		height: 100rpx;
		margin-top: 30rpx;
		padding-left: 20rpx;
		margin-left: 20rpx;
		// background-color: green;
		font-size: 38rpx;
		border-bottom: 4rpx solid #cccccc;
		outline: none;
		color: white;
	}
	.del{
		width: 120rpx;
		height: 80rpx;
		font-size: 32rpx;
		color: white;
		font-weight: 700;
		margin-top: 40rpx;
		border-radius: 20rpx;
	}
	.selectClass{
		background: #e7e4dc;
	}
	.line-through{
		color: rgba(255, 255, 255, 0.5);
		text-decoration: line-through rgba(255, 255, 255, 0.8);
	}
</style>
