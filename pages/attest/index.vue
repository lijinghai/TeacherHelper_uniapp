<!-- 企业认证页面
@author: lijing
@email: lijinghailjh@163.com
@Date: 2021 8 11
 -->
<template>
	<view>
		<view>
			<cu-custom bgColor="bg-gradual-blue" :isBack="true">
				<block slot="backText">返回</block>
				<block slot="content">企业认证</block>
			</cu-custom>
		</view>

		<!-- 内容 -->
		<!-- 步骤条 -->
		<view class="bg-white padding">
			<view class="cu-steps">
				<view class="cu-item" :class="index>num?'':'text-blue'" v-for="(item,index) in numList" :key="index">
					<text class="num" :class="index==2?'':''" :data-index="index + 1"></text> {{item.name}}
				</view>
			</view>
		</view>


		<view class="wrap">
			<u-form :model="model" :rules="rules" ref="uForm" :errorType="errorType">
				
				<!-- 营业执照 -->
				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" left-icon="photo-fill"
					:label-position="labelPosition" label="*事业单位证明/营业执照" prop="photo" label-width="330">
					<u-upload width="160" height="160"></u-upload>
				</u-form-item>

				<!-- 主体名称 -->
				<u-form-item  label-width="200"
					:label-position="labelPosition" label="*主体名称" prop="name">
					<u-input :border="border" placeholder="请输入证件一致名称(无字号个体户填经营者姓名)" v-model="model.name" type="text"></u-input>
				</u-form-item>

				<!-- 统一社会信用代码 -->
				<u-form-item label-width="300" :label-position="labelPosition" label="*统一社会信用代码" prop="name">
					<u-input :border="border" placeholder="请输入18位统一信用代码" v-model="model.name" type="text"></u-input>
				</u-form-item>

				<!-- 机构类型 -->
				<u-form-item :label-position="labelPosition" label="机构类型" prop="sex" label-width="200">
					<u-input :border="border" type="select" :select-open="actionSheetShow" v-model="model.sex"
						placeholder="请选择机构类型" @click="actionSheetShow = true"></u-input>
				</u-form-item>

				<!--法定代表人 -->
				<u-form-item label-width="200" :label-position="labelPosition" label="*法定代表人" prop="name">
					<u-input :border="border" placeholder="请输入法定代表人姓名" v-model="model.name" type="text"></u-input>
				</u-form-item>


			</u-form>
			<view class="agreement">
				<u-checkbox v-model="check" @change="checkboxChange"></u-checkbox>
				<view class="agreement-text">
					勾选代表同意认证的版权协议
				</view>
			</view>
			<u-button @click="submit" type="success">提交</u-button>
			<u-action-sheet :list="actionSheetList" v-model="actionSheetShow" @click="actionSheetCallback">
			</u-action-sheet>
			<u-select mode="single-column" :list="selectList" v-model="selectShow" @confirm="selectConfirm"></u-select>
			<u-picker mode="region" v-model="pickerShow" @confirm="regionConfirm"></u-picker>
			<u-verification-code seconds="60" ref="uCode" @change="codeChange"></u-verification-code>

		</view>


		<view class="padding bg-white">
			<!-- <view class="text-center padding">我于杀戮之中绽放 ,亦如黎明中的花朵</view> -->
			<view class="text-right padding text-blue" @click="goNext">下一步</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {

				model: {
					name: '',
					sex: '',
					likeFruit: '',
					intro: '',
					payType: '支付宝',
					agreement: false,
					region: '',
					goodsType: '',
					phone: '',
					code: '',
					password: '',
					rePassword: '',
					remember: false,
					photo: ''
				},
				selectList: [{
						value: '电子产品',
						label: '电子产品'
					},
					{
						value: '服装',
						label: '服装'
					},
					{
						value: '工艺品',
						label: '工艺品'
					}
				],

				rules: {
					name: [{
							required: true,
							message: '请输入姓名',
							trigger: 'blur',
						},
						{
							min: 3,
							max: 5,
							message: '姓名长度在3到5个字符',
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
					sex: [{
						required: true,
						message: '请选择性别',
						trigger: 'change'
					}, ],
					intro: [{
							required: true,
							message: '请填写简介'
						},
						{
							min: 5,
							message: '简介不能少于5个字',
							trigger: 'change',
						},
						// 正则校验示例，此处用正则校验是否中文，此处仅为示例，因为uView有this.$u.test.chinese可以判断是否中文
						{
							pattern: /^[\u4e00-\u9fa5]+$/gi,
							message: '简介只能为中文',
							trigger: 'change',
						},
					],
					likeFruit: [{
						required: true,
						message: '请选择您喜欢的水果',
						trigger: 'change',
						type: 'array',
					}],
					payType: [{
						required: true,
						message: '请选择任意一种支付方式',
						trigger: 'change',
					}],
					region: [{
						required: true,
						message: '请选择地区',
						trigger: 'change',
					}],
					goodsType: [{
						required: true,
						message: '请选择商品类型',
						trigger: 'change',
					}],
					phone: [{
							required: true,
							message: '请输入手机号',
							trigger: ['change', 'blur'],
						},
						{
							validator: (rule, value, callback) => {
								// 调用uView自带的js验证规则，详见：https://www.uviewui.com/js/test.html
								return this.$u.test.mobile(value);
							},
							message: '手机号码不正确',
							// 触发器可以同时用blur和change，二者之间用英文逗号隔开
							trigger: ['change', 'blur'],
						}
					],
					code: [{
							required: true,
							message: '请输入验证码',
							trigger: ['change', 'blur'],
						},
						{
							type: 'number',
							message: '验证码只能为数字',
							trigger: ['change', 'blur'],
						}
					],
					password: [{
							required: true,
							message: '请输入密码',
							trigger: ['change', 'blur'],
						},
						{
							// 正则不能含有两边的引号
							pattern: /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]+\S{5,12}$/,
							message: '需同时含有字母和数字，长度在6-12之间',
							trigger: ['change', 'blur'],
						}
					],
					rePassword: [{
							required: true,
							message: '请重新输入密码',
							trigger: ['change', 'blur'],
						},
						{
							validator: (rule, value, callback) => {
								return value === this.model.password;
							},
							message: '两次输入的密码不相等',
							trigger: ['change', 'blur'],
						}
					],
				},
				border: false,
				check: false,
				selectStatus: 'close',
				checkboxList: [{
						name: '荔枝',
						checked: false,
						disabled: false
					},
					{
						name: '香蕉',
						checked: false,
						disabled: false
					},
					{
						name: '橙子',
						checked: false,
						disabled: false
					},
					{
						name: '草莓',
						checked: false,
						disabled: false
					}
				],
				radioList: [{
						name: '支付宝',
						checked: true,
						disabled: false
					},
					{
						name: '微信',
						checked: false,
						disabled: false
					},
					{
						name: '银联',
						checked: false,
						disabled: false
					},
					{
						name: '现金',
						checked: false,
						disabled: false
					}
				],
				radio: '支付宝',
				actionSheetList: [{
						text: '政府/事业单位'
					},
					{
						text: '企业'
					},
					{
						text: '个体工商户'
					},
					{
						text: '其他组织'
					}
				],
				actionSheetShow: false,
				pickerShow: false,
				selectShow: false,
				radioCheckWidth: 'auto',
				radioCheckWrap: false,
				labelPosition: 'left',
				codeTips: '',
				errorType: ['message'],




				// 步骤条
				numList: [{
					name: '企业认证'
				}, {
					name: '绑定正式章'
				}, {
					name: '个人认证'
				}, {
					name: '完成'
				}, ],
				num: -1
			};
		},
		onLoad() {
			this.NumSteps(1)
		},
		computed: {
			borderCurrent() {
				return this.border ? 0 : 1;
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		// 返回页面跳转
		onUnload() {
			uni.switchTab({
				url: '../index/index'
			})
		},
		methods: {


			// 表单提交
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						if (!this.model.agreement) return this.$u.toast('请勾选协议');
						console.log('验证通过');
						uni.navigateTo({
							url: './formal'
						})
					} else {
						console.log('验证失败');
					}
				});
			},

			// 点击actionSheet回调
			actionSheetCallback(index) {
				uni.hideKeyboard();
				this.model.sex = this.actionSheetList[index].text;
			},
			// checkbox选择发生变化
			checkboxGroupChange(e) {
				this.model.likeFruit = e;
			},
			// radio选择发生变化
			radioGroupChange(e) {
				this.model.payType = e;
			},
			// 勾选版权协议
			checkboxChange(e) {
				this.model.agreement = e.value;
			},
			// 选择地区回调
			regionConfirm(e) {
				this.model.region = e.province.label + '-' + e.city.label + '-' + e.area.label;
			},
			// 选择商品类型回调
			selectConfirm(e) {
				this.model.goodsType = '';
				e.map((val, index) => {
					this.model.goodsType += this.model.goodsType == '' ? val.label : '-' + val.label;
				})
			},
			borderChange(index) {
				this.border = !index;
			},
			radioCheckboxChange(index) {
				if (index == 0) {
					this.radioCheckWrap = false;
					this.radioCheckWidth = 'auto';
				} else if (index == 1) {
					this.radioCheckWrap = true;
					this.radioCheckWidth = 'auto';
				} else if (index == 2) {
					this.radioCheckWrap = false;
					this.radioCheckWidth = '50%';
				}
			},
			labelPositionChange(index) {
				this.labelPosition = index == 0 ? 'left' : 'top';
			},
			codeChange(text) {
				this.codeTips = text;
			},
			// 获取验证码
			getCode() {
				if (this.$refs.uCode.canGetCode) {
					// 模拟向后端请求验证码
					uni.showLoading({
						title: '正在获取验证码',
						mask: true
					})
					setTimeout(() => {
						uni.hideLoading();
						// 这里此提示会被this.start()方法中的提示覆盖
						this.$u.toast('验证码已发送');
						// 通知验证码组件内部开始倒计时
						this.$refs.uCode.start();
					}, 2000);
				} else {
					this.$u.toast('倒计时结束后再发送');
				}
			},
			errorChange(index) {
				if (index == 0) this.errorType = ['message'];
				if (index == 1) this.errorType = ['toast'];
				if (index == 2) this.errorType = ['border-bottom'];
				if (index == 3) this.errorType = ['border'];
			},





			// 跳转到下一页面
			goNext() {
				uni.navigateTo({
					url: './formal'
				})
			},

			// 下一个界面激活步骤条
			myCliack() {
				//当前页
				var pages = getCurrentPages();
				//上个页面
				var beforePage = pages[pages.length - 2];
				beforePage.$vm.NumSteps(1)

				console.log("点击了")

			},

			NumSteps(step) {
				console.log("step======>" + step)
				if (step != null) {
					this.num = step - 1
					console.log('如果正处于当前第' + step + '步骤,则使用这个图片===>' + step)
				} else if (step > this.num) {
					console.log('如果该步骤比当前步骤大 也就是还没到,则展示这个图片')
				} else {
					console.log('最后就是已经过了该步骤应该展示的图片')
				}
				// this.num= this.num == this.numList.length - 1 ? 0 : this.num + 1
			}

		}
	}
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
