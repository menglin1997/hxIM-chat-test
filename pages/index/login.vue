<template>
	<view class="content">
		<view class="">
			<!-- 当前用户在线状态：{{ status }} -->
		</view>
		<view class="" v-for="(item,index) in messageList" :key="index">
			当前用户在线未读消息：{{ item.data}}
		</view>
		<input type="text" value="" v-model="message" />
		<button @tap="login">登录</button>
		<button @tap="logOut">退出登录</button>
		<view @tap="togyb2">给sads1发消息</view>
		<button type="default"></button>
		<button type="default" @tap="getFile">发送图片消息</button>
		
	</view>
</template>

<script>
	let WebIM = require("../../utils/WebIM")["default"];
	let __test_account__, __test_psword__;
	let disp = require("../../utils/broadcast"); // __test_account__ = "easezy";
	export default {
		data() {
			return {
				title: 'Hello',
				status:'离线',
				messageList: getApp().globalData.txt,
				message:'发送一条消息',
				userInfo: {
					username:null,
					password:null,
				}
			}
		},
		onLoad(val) {
				let me = this
				console.log(val,'val')
				this.login()
				console.log( getApp().globalData.txt,' getApp().globalData')
				this.userInfo.username = val.login
				this.userInfo.password = val.password
				disp.on("em.error.passwordErr", function (e) {
				  uni.showToast({title: "用户名或密码错误",icon:'none'});
				  // 登陆失败创建用户
					// var options = { 
				 //      username: 'sads1',
				 //      password: 'sads123456',
				 //      nickname: 'sads1',
				 //      appKey: WebIM.config.appkey,
				 //      success: function () { },
				 //      error: function () { },
				 //      apiUrl: WebIM.config.apiURL
				 //    }; 
				 //    WebIM.conn.registerUser(options);
					
				});
				// setTimeout(() => {
				// 	this.login()
				// },500)
				
		},
		methods: {
			// 退出登录
			logOut() {
				console.log(WebIM,'webim')
				WebIM.conn.close();
			},
			// 连接并登录
			login() {
				let thas = this
				getApp().globalData.conn.open({
				  apiUrl: WebIM.config.apiURL,
				  user: 'sads',
				  pwd: 'sads123456',
				  grant_type: 'password',
				  appKey: WebIM.config.appkey,
				  isMultiLoginSessions:false
				});
			},
			getFile (e) {
				   var thas = this
				//    var str = e.target
				//    console.log(str)
				   uni.chooseImage({
						count: 6, //默认9
						sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
						sourceType: ['album'], //从相册选择
						success: function (res) {
							console.log(res,'res')
							let blob = res.tempFilePaths[0]
							console.log(typeof blob,'typeof')
						// var width = res.width;
						// var height = res.height;
						// var index = res.path.lastIndexOf(".");
						// var filetype = ~index && res.path.slice(index + 1) || "";
						// console.log(filetype,'filetype')
						//   var id = conn.getUniqueId();                   // 生成本地消息id
						// var url = URL.createObjectURL(res.tempFilePaths[0]);
             
							
						// 	var url = res.tempFilePaths[0];
							var id = WebIM.conn.getUniqueId();             // 生成本地消息id
							var msg = new WebIM.message('img', id);  // 创建图片消息
							msg.set({
								apiUrl: WebIM.config.apiURL,
								file: {data: 1, url: 'http://www.ichong123.com/uploads/2014/08/z12.jpg'},
								to: 'sads1',                      // 接收消息对象
								from: 'sads',                      // 发送消息对象
								roomType: false,
								onFileUploadError: function (error) {
									console.log('Error');
								},
								onFileUploadComplete: function (data) {
									console.log('Complete');
								},
								success: function (id) {
									console.log('Success');
								}
							});
							console.log(msg,'msg')
							WebIM.conn.send(msg.body);
							// thas.upLoadImage(res);
						// 	let str = res.tempFilePaths
						// 	console.log(res.tempFilePaths);
						// 	 var id = WebIM.conn.getUniqueId();                  // 生成本地消息id
						// 			   var msg = new WebIM.message('img', id);        // 创建图片消息
						// 			//    var input = document.getElementById('image');  // 选择图片的input
						// 			   var file = WebIM.utils.getFileUrl(str);      // 将图片转化为二进制文件
						// 			   console.log(file,'file')
						// 			   var allowType = {
						// 			       'jpg': true,
						// 			       'gif': true,
						// 			       'png': true,
						// 			       'bmp': true
						// 			   };
						// 			   if (file.filetype.toLowerCase() in allowType) {
						// 			       var option = {
						// 			           apiUrl: WebIM.config.apiURL,
						// 			           file: file,
						// 			           ext: {
						// 			               file_length: file.data.size       // 文件大小（有没有都不影响）
						// 			           },
						// 			           to: '发送对象的id',   // 接收消息对象
						// 			           roomType: false,
						// 			           onFileUploadError: function () {      // 消息上传失败
						// 			               console.log('上传失败');
						// 			           },
						// 			           onFileUploadComplete: function () {   // 消息上传成功
						// 			               console.log('上传成功');
						// 			           },
						// 			           success: function () {                // 消息发送成功
						// 			               console.log('发送成功');
						// 			           },
						// 			           flashUpload: WebIM.flashUpload
						// 			       };
						// 			       msg.set(option);
						// 				    WebIM.conn.send(msg.body);
						// 			   }
						}
					});
				 
				 },
			

			 upLoadImage(res) {
				var me = this;
				var tempFilePaths = res.tempFilePaths;
				var token = WebIM.conn.context.accessToken;
				var filetype = (res.tempFiles[0].type).split('/')
				console.log(filetype,'filetype')
				filetype = filetype[1]
				console.log(filetype,'filetype')

				uni.getImageInfo({
					src: res.tempFilePaths[0],
					success(infoRes) {
						console.log(infoRes, 'res------')
						var allowType = {
							jpg: true,
							jpeg: true,
							gif: true,
							png: true,
							bmp: true
						};
						
						// if (filetype.toLowerCase() in allowType) {
						// 	uni.uploadFile({
						// 		url: "https://a1.easemob.com/" + str[0] + "/" + str[1] + "/chatfiles",
						// 		filePath: tempFilePaths[0],
						// 		name: "file",
						// 		header: {
						// 			"Content-Type": "multipart/form-data",
						// 			Authorization: "Bearer " + token
						// 		},

						// 		success(res) {
						// 			console.log(res,'res')
						// 			var data = res.data;
						// 			var dataObj = JSON.parse(data);
						// 			var id = WebIM.conn.getUniqueId(); // 生成本地消息 id
						// 			console.log('dataObj>>',dataObj);
									
						// 			var msg = new WebIM.message('png', id);
						// 			var file = {
						// 				type: 'png',
						// 				size: {
						// 					width: width,
						// 					height: height
						// 				},
						// 				url: dataObj.uri + "/" + dataObj.entities[0].uuid,
						// 				filetype: filetype,
						// 				filename: tempFilePaths[0]
						// 			};
						// 			msg.set({
						// 				apiUrl: WebIM.config.apiURL,
						// 				body: file,
						// 				from: 'sads',
						// 				to: 'sads1',
						// 				roomType: false,
						// 				chatType: me.chatType,
						// 				success: function (argument) {
						// 					disp.fire('em.chat.sendSuccess', id);
						// 				}
						// 			});

						// 			// if (me.chatType == msgType.chatType.CHAT_ROOM) {
						// 			// 	msg.setGroup("groupchat");
						// 			// }
						// 			console.log(msg.body,'发送的图片消息')

						// 			WebIM.conn.send(msg.body);
						// 			// let obj = {
						// 			// 	msg: msg,
						// 			// 	type: msgType.IMAGE
						// 			// }
						// 			// console
						// 			// me.saveSendMsg(obj);
						// 		}
						// 	});
						// // }
					}
				});
			},
			
			// 发送图片消息
			sendImg() {
				
				let thas = this
				var id = WebIM.conn.getUniqueId();                 // 生成本地消息id
				var msg = new WebIM.message('txt', id);      // 创建文本消息
				    msg.set({
				        msg: thas.message,                  // 消息内容
				        to: 'sads1',                          // 接收消息对象（用户id）
				        roomType: false,
						from:'sads',
				        ext: {
				            key: 11,
				            key2: {
				                key3: 22
				            }
				        },                                  //扩展消息
				        success: function (id, serverMsgId) {
							uni.showToast({
								title:'发送成功',
								icon:'success'
							})
				            console.log('send private text Success',id,serverMsgId,'发送成功');  
				        },                                       // 对成功的相关定义，sdk会将消息id登记到日志进行备份处理
				        fail: function(e){
				            console.log("Send private text error");  
				        }                                        // 对失败的相关定义，sdk会将消息id登记到日志进行备份处理
				    });
					console.log(msg,'msg')
				    WebIM.conn.send(msg.body);
			},
			
			// 发文本消息
			togyb2() {
				let thas = this
				var id = WebIM.conn.getUniqueId();                 // 生成本地消息id
				    var msg = new WebIM.message('txt', id);      // 创建文本消息
				    msg.set({
				        msg: thas.message,                  // 消息内容
				        to: 'sads1',                          // 接收消息对象（用户id）
				        roomType: false,
						from:'sads',
				        ext: {
				            key: 11,
				            key2: {
				                key3: 22
				            }
				        },                                  //扩展消息
				        success: function (id, serverMsgId) {
							uni.showToast({
								title:'发送成功',
								icon:'success'
							})
				            console.log('send private text Success',id,serverMsgId,'发送成功');  
				        },                                       // 对成功的相关定义，sdk会将消息id登记到日志进行备份处理
				        fail: function(e){
				            console.log("Send private text error");  
				        }                                        // 对失败的相关定义，sdk会将消息id登记到日志进行备份处理
				    });
					console.log(msg,'msg')
				    WebIM.conn.send(msg.body);
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
