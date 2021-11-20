<!-- 编辑验证码页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 8 4
-->
<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">编辑验证码页面</block>
		</cu-custom>

		<view class="wrap">

			<u-form :model="model" :rules="rules" ref="uForm" :errorType="errorType">

				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" left-icon="account" label-width="200"
					:label-position="labelPosition" label="姓名" prop="name">
					<u-input :border="border" placeholder="请输入经办人姓名" v-model="model.name" type="text"></u-input>
				</u-form-item>

				<u-form-item :label-position="labelPosition" label="盖章单位名称" prop="company" label-width="200">
					<u-input type="textarea" :border="border" placeholder="请填写盖章单位名称" v-model="model.company" />
				</u-form-item>

				<u-form-item :label-position="labelPosition" label="时间" prop="time" label-width="200">
					<u-input :border="border" type="select" :select-open="pickerShowTime" v-model="model.time"
						placeholder="请选择时间" @click="pickerShowTime = true"></u-input>
				</u-form-item>

				<u-form-item :label-position="labelPosition" label="所在地区" prop="region" label-width="200">
					<u-input :border="border" type="select" :select-open="pickerShow" v-model="model.region"
						placeholder="请选择地区" @click="pickerShow = true"></u-input>
				</u-form-item>

				<u-form-item :label-position="labelPosition" label="详细地址" prop="intro" label-width="200">
					<u-input type="textarea" :border="border" placeholder="请填写详细地址" v-model="model.intro" />
				</u-form-item>

				<u-form-item :label-position="labelPosition" label="上传图片" prop="photo" label-width="150">
					<u-upload width="160" height="160"></u-upload>
				</u-form-item>

			</u-form>

			<view class="agreement">
				<u-checkbox v-model="check" @change="checkboxChange"></u-checkbox>
				<view class="agreement-text">
					勾选代表同意隐私的版权协议
				</view>
			</view>

			<u-button @click="submit" type="success">重新提交</u-button>


			<u-select mode="single-column" :list="selectList" v-model="selectShow" @confirm="selectConfirm"></u-select>
			<u-picker mode="region" v-model="pickerShow" @confirm="regionConfirm"></u-picker>
			<u-picker mode="time" :params="params"  end-year="2030"
				v-model="pickerShowTime" @confirm="timefirm"></u-picker>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			let that = this;
			return {

				// time: '',

				params: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: true,
					second: true,
					timestamp: true
				},

				model: {
					// 经办人姓名
					name: '',
					// 盖章单位
					company: '',
					// 详细地址
					intro: '',
					// 版权
					agreement: false,
					// 所在地址
					region: '',
					//所在时间
					time: '',
					// 图片
					photo: ''
				},

				// 规则
				rules: {
					// 经办人姓名
					name: [{
							required: true,
							message: '请输入姓名',
							trigger: 'blur',
						},
						{
							min: 2,
							max: 5,
							message: '姓名长度在2到5个字符',
							trigger: ['change', 'blur'],
						},
						{
							// 此为同步验证，可以直接返回true或者false，如果是异步验证，稍微不同，见下方说明
							validator: (rule, value, callback) => {
								// 调用uView自带的js验证规则，详见：https://www.uviewui.com/js/test.html
								return this.$u.test.chinese(value);
							},
							message: '姓名必须为中文',
							// 触发器可以同时用blur和change，二者之间用英文逗号隔开
							trigger: ['change', 'blur'],
						},
						// 异步验证，用途：比如用户注册时输入完账号，后端检查账号是否已存在
						// {
						// 	trigger: ['blur'],
						// 	// 异步验证需要通过调用callback()，并且在里面抛出new Error()
						// 	// 抛出的内容为需要提示的信息，和其他方式的message属性的提示一样
						// 	asyncValidator: (rule, value, callback) => {
						// 		this.$u.post('/ebapi/public_api/index').then(res => {
						// 			// 如果验证出错，需要在callback()抛出new Error('错误提示信息')
						// 			if(res.error) {
						// 				callback(new Error('姓名重复'));
						// 			} else {
						// 				// 如果没有错误，也要执行callback()回调
						// 				callback();
						// 			}
						// 		})
						// 	},
						// }
					],

					// 盖章单位
					company: [{
							required: true,
							message: '请填写盖章单位名称'
						},
						{
							min: 3,
							message: '盖章单位名称不能少于3个字',
							trigger: 'change',
						},
						// 正则校验示例，此处用正则校验是否中文，此处仅为示例，因为uView有this.$u.test.chinese可以判断是否中文
						{
							pattern: /^[\u4e00-\u9fa5]+$/gi,
							message: '盖章单位名称只能为中文',
							trigger: 'change',
						},
					],

					// 详细地址
					intro: [{
							required: true,
							message: '请填写详细地址'
						},
						{
							min: 5,
							message: '详细地址不能少于5个字',
							trigger: 'change',
						},
						// 正则校验示例，此处用正则校验是否中文，此处仅为示例，因为uView有this.$u.test.chinese可以判断是否中文
						// {
						// 	pattern: /^[\u4e00-\u9fa5]+$/gi,
						// 	message: '详细地址只能为中文',
						// 	trigger: 'change',
						// },
					],

					// 所在地区
					region: [{
						required: true,
						message: '请选择地区',
						trigger: 'change',
					}],

					// 所在时间
					time: [{
						required: true,
						message: '请选择时间',
						trigger: 'change',
					}],


				},
				border: false,
				check: false,
				selectStatus: 'close',


				actionSheetShow: false,
				pickerShow: false,
				pickerShowTime: false,
				selectShow: false,
				labelPosition: 'left',
				errorType: ['message'],
			};
		},
		onLoad() {

		},
		computed: {

		},

		// 规则校验
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		methods: {

			// 提交
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						if (!this.model.agreement) return this.$u.toast('请勾选协议');
						console.log('验证通过');
						console.log(valid)
						
						const _this = this // 获取此时的this为一个常量，防止下面请求回调改变出错
						console.log("表单提交的内容===>")
						console.log(_this.model)
						uni.navigateBack({
							url:'pages/getCode/index'
						})
						this.$tip.success("提交成功")
						
					} else {
						console.log('验证失败');
					}
				});
			},


			// 勾选版权协议
			checkboxChange(e) {
				this.model.agreement = e.value;
			},

			// 选择地区回调
			regionConfirm(e) {
				this.model.region = e.province.label + '-' + e.city.label + '-' + e.area.label;
				console.log("选择的地区===>")
				console.log(this.model.region)
			},

			// 选择时间回调
			timefirm(e) {
				this.model.time = '';
				if (this.params.year) this.model.time += e.year;
				if (this.params.month) this.model.time += '-' + e.month;
				if (this.params.day) this.model.time += '-' + e.day;
				if (this.params.hour) this.model.time += ' ' + e.hour;
				if (this.params.minute) this.model.time += ':' + e.minute;
				if (this.params.second) this.model.time += ':' + e.second;

				console.log("选择的时间===>")
				console.log(this.model.time)
			}
		}
	};
</script>

<style scoped lang="scss">
	.wrap {
		padding: 30rpx;
	}

	.agreement {
		display: flex;
		align-items: center;
		margin: 40rpx 0;

		.agreement-text {
			padding-left: 8rpx;
			color: $u-tips-color;
		}
	}
</style>

