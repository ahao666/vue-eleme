


1. sass 使用
		
		1. 安装sass的依赖包

			npm install --save-dev sass-loader
			//sass-loader依赖于node-sass
			npm install --save-dev node-sass

		2. 在build文件夹下的webpack.base.conf.js的rules里面添加配置
			{
			  test: /\.sass$/,
			  loaders: ['style', 'css', 'sass']
			}

		3. 在APP.vue中修改style标签
			<style lang="scss">


2. axios 使用 

		axios 改写为 Vue 的原型属性 
		
		import axios from 'axios'
		Vue.prototype.$ajax= axios	
	
		this.$ajax.get('api/getNewsList').then((response)=>{
	        this.newsList=response.data.data;
	      }).catch((response)=>{
	        console.log(response);
	      })