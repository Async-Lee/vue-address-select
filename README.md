# vue-address-select

> A Vue.js project

## Build Setup

``` bash

超级厉害的vue省市区三级联动模块,自带数据。

npm install --save vue-address-select

<vue-address-select @getAddress="addressRes" :addressVal="address"></vue-address-select>

import vueAddressSelect from 'vue-address-select'
export default {
	data(){
		return {
			address:0,
		}
	},
	mthods(){
		addressRes(val){
            this.address = val;
        },
	}
}

```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
