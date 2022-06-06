# 验证码登录

```javascript
1.自定义一个方法，用于禁止多次点击获取验证码
function noMultipleClicks(methods) {
    let that = this;
    
    if (that.noClick) {
        that.noClick= false;
        methods();
        setTimeout(function () {
            that.noClick= true;
        }, 2000)
    } else {
        console.log("请稍后点击")
    }
}
2.获取验证码
//获取验证码
		getCode(){
			if(!this.mobile||!/^[1][0-9]{10}$/.test(this.mobile)){
				uni.showToast({
					title:'请输入正确手机号',
					icon:'none'
				})
				return;
			}
			uni.showLoading()
			this.$http.post('发送获取验证码的请求',{phone:this.mobile}).then(res=>{
				uni.hideLoading()
				if(res.code==1){
					const TIME_COUNT = 60;
					if (!this.timer) {
						this.count = TIME_COUNT;
						this.timer = setInterval(() => {
							if (this.count > 0 && this.count <= TIME_COUNT) {
								this.count--;
							} else {
								clearInterval(this.timer);
								this.timer = null;
							}
						}, 1000)
					}
					uni.showToast({
						title:res.msg
					})
				}else{
					uni.showToast({
						title:res.msg,
						icon:'none'
					})
				}
			})
		},
```



# 微信支付

