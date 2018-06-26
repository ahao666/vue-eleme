


## sass 使用
		
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


## axios 使用 

		axios 改写为 Vue 的原型属性 
		
		import axios from 'axios'
		Vue.prototype.$ajax= axios	
	
		this.$ajax.get('api/getNewsList').then((response)=>{
	        this.newsList=response.data.data;
	      }).catch((response)=>{
	        console.log(response);
	      })






## vue：Uncaught TypeError：无法读取未定义的属性   

	[https://stackoverflow.com/questions/41051972/vue-uncaught-typeerror-cannot-read-property-of-undefined](https://stackoverflow.com/questions/41051972/vue-uncaught-typeerror-cannot-read-property-of-undefined "具体答案")
		
		在本地开发时，我经常看到警告Uncaught TypeError: Cannot read property ... of undefined，但HTML可以成功呈现。
		但是，将HTML部署到带有npm run build命令的Netlify时，将无法呈现HTML 。所以我必须认真对待这个警告。
		
		我从这里了解到，这是因为“组件渲染时数据不完整，而是例如从API加载数据”。解决方案是“ v-if仅在数据加载完成后才使用该模板部分”。

		<div class="goods" v-if="allData.goods">
        	<div class="menu-wrapper">{{getallData.goods[0].name}}</div>
        	<div class="foods-wrapper"></div>
    	</div>

		v-if每个拥有AJAX调用的组件放在首位。





## created mounted updated 



## $nextTick()