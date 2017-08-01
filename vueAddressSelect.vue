<template>
    <div class="addressSelect" >
        <ul @touchstart="touchStart($event,'province')"  @touchmove="touchMove($event,'province')"  @touchend="touchEnd($event,'province')" :style="provinceStyle" :class="[{'selectAni':addSelect}]">
            <li v-for="(item,index) in addressData" :class="[{'addSelectActive':index == provinceIndex}]" :key="index">{{item.name}}</li>
        </ul>  
        <ul @touchstart="touchStart($event,'city')" @touchmove="touchMove($event,'city')" @touchend="touchEnd($event,'city')" :style="cityStyle" :class="[{'selectAni':addSelect}]">
            <li v-for="(item,index) in addressData[provinceIndex].city" :key="index" :class="[{'addSelectActive':index == cityIndex}]" @touchmove="touchMove" @touchstart="touchStart" @click="districtSelect(index)">{{item.name}}</li>
        </ul> 
         <ul @touchstart="touchStart($event,'district')" @touchmove="touchMove($event,'district')" @touchend="touchEnd($event,'district')" :style="districtStyle" :class="[{'selectAni':addSelect}]">
            <li v-for="(item,index) in addressData[provinceIndex].city[cityIndex].area" :class="[{'addSelectActive':index == districtIndex}]" :key="index" >{{item}}</li> 
        </ul>
    </div>
</template>

<script>
import {addressData} from './static/js/address';
export default {
    name:'vueAddressSelect',
    data(){
        return {
            provinceStyle:{
                WebkitTransform:'translate3d(0px,0px,0px)',
            },
            cityStyle:{
                WebkitTransform:'translate3d(0px,0px,0px)',
            },
            districtStyle:{
                WebkitTransform:'translate3d(0px,0px,0px)',
            },
            startTop:0,
            addressData,
            provinceIndex:0,
            cityIndex:0,
            districtIndex:0,
            translateY:0,
            maxScroll:0,
            addHeight:0,
            addSelect:false,
            val:{'provinceVal':addressData[0].name,
                'cityVal':addressData[0].city[0].name,
                'areaVal':addressData[0].city[0].area[0],
            }
        }
    },
    props:['addressVal'],
    methods:{
        touchStart(e,val){
            e.preventDefault();
            this.addSelect = false;
            this.addHeight = e.currentTarget.children[0].offsetHeight
            this.maxScroll =  this.addHeight * e.currentTarget.children.length
            this.startTop = e.targetTouches[0].pageY;
            switch(val)
            {
                case 'province':
                    this.translateY = parseInt(this.provinceStyle.WebkitTransform.slice(this.provinceStyle.WebkitTransform.indexOf(',')+1,this.provinceStyle.WebkitTransform.lastIndexOf(',')));
                    break;
                case 'city':
                    this.translateY = parseInt(this.cityStyle.WebkitTransform.slice(this.cityStyle.WebkitTransform.indexOf(',')+1,this.cityStyle.WebkitTransform.lastIndexOf(',')));
                    break;
                case 'district':
                    this.translateY = parseInt(this.districtStyle.WebkitTransform.slice(this.districtStyle.WebkitTransform.indexOf(',')+1,this.districtStyle.WebkitTransform.lastIndexOf(',')));
                    break;
                default:
                    break;
            }
        },
        touchMove(e,val){
            e.preventDefault();
            switch(val)
            {
                case 'province':
                    if((e.targetTouches[0].pageY - this.startTop  + this.translateY) >  0){
                        this.provinceStyle.WebkitTransform = 'translate3d(0px,0px,0px)';
                    }else if((e.targetTouches[0].pageY - this.startTop  + this.translateY) < -(this.maxScroll - this.addHeight)){
                        this.provinceStyle.WebkitTransform = 'translate3d(0px,'+ -(this.maxScroll - this.addHeight) +'px,0px)';
                    }else{
                        this.provinceStyle.WebkitTransform = 'translate3d(0px,' + (e.targetTouches[0].pageY - this.startTop  + this.translateY) + 'px,0px)';
                    }
                    
                    break;
                case 'city':
                    if((e.targetTouches[0].pageY - this.startTop  + this.translateY) >  0){
                        this.cityStyle.WebkitTransform = 'translate3d(0px,0px,0px)'
                    }else if((e.targetTouches[0].pageY - this.startTop  + this.translateY) < -(this.maxScroll  - this.addHeight)){
                        this.cityStyle.WebkitTransform = 'translate3d(0px,'+ -(this.maxScroll - this.addHeight) +'px,0px)';
                    }else{
                        this.cityStyle.WebkitTransform = 'translate3d(0px,' + (e.targetTouches[0].pageY - this.startTop  + this.translateY) + 'px,0px)';
                    }
                    break;
                case 'district':
                    if((e.targetTouches[0].pageY - this.startTop  + this.translateY) >  0){
                        this.districtStyle.WebkitTransform = 'translate3d(0px,0px,0px)'
                    }else if((e.targetTouches[0].pageY - this.startTop  + this.translateY) < -(this.maxScroll - this.addHeight)){
                        this.districtStyle.WebkitTransform = 'translate3d(0px,'+ -(this.maxScroll - this.addHeight) +'px,0px)';
                    }else{
                        this.districtStyle.WebkitTransform = 'translate3d(0px,' + (e.targetTouches[0].pageY - this.startTop  + this.translateY) + 'px,0px)';
                    }
                    break;
                default:
                    break;
            }
                
           
        },
        touchEnd(e,val){
            e.preventDefault();
            this.addSelect = true;
            switch(val)
            {
                case 'province':
                    let provinceTranslateY = parseInt(this.provinceStyle.WebkitTransform.slice(this.provinceStyle.WebkitTransform.indexOf(',')+1,this.provinceStyle.WebkitTransform.lastIndexOf(',')));
                    this.provinceIndex = -Math.round(provinceTranslateY/this.addHeight)
                    this.provinceStyle.WebkitTransform = 'translate3d(0px,' + (Math.round(provinceTranslateY/this.addHeight) * this.addHeight) + 'px,0px)';
                    this.cityStyle.WebkitTransform = this.districtStyle.WebkitTransform = 'translate3d(0px,0px,0px)';
                    this.cityIndex = this.districtIndex = 0;
                    break;
                case 'city':
                    let cityTranslateY = parseInt(this.cityStyle.WebkitTransform.slice(this.cityStyle.WebkitTransform.indexOf(',')+1,this.cityStyle.WebkitTransform.lastIndexOf(',')));
                    this.cityIndex = -Math.round(cityTranslateY/this.addHeight)
                    this.cityStyle.WebkitTransform = 'translate3d(0px,' + (Math.round(cityTranslateY/this.addHeight) * this.addHeight) + 'px,0px)';
                    this.districtStyle.WebkitTransform = 'translate3d(0px,0px,0px)'
                    this.districtIndex = 0;
                    break;
                case 'district':
                    let districtTranslateY = parseInt(this.districtStyle.WebkitTransform.slice(this.districtStyle.WebkitTransform.indexOf(',')+1,this.districtStyle.WebkitTransform.lastIndexOf(',')));
                    this.districtIndex = -Math.round(districtTranslateY/this.addHeight)
                    this.districtStyle.WebkitTransform = 'translate3d(0px,' + (Math.round(districtTranslateY/this.addHeight) * this.addHeight) + 'px,0px)';
                    break;
                default:
                    break;
            }
            this.val.provinceVal = this.addressData[this.provinceIndex].name;
            this.val.cityVal = this.addressData[this.provinceIndex].city[this.cityIndex].name;
            this.val.areaVal = this.addressData[this.provinceIndex].city[this.cityIndex].area[this.districtIndex];
            this.$emit('getAddress',this.val)
        }
    },
    created(){
        this.$emit('getAddress',this.val)
        if(this.addressVal){
            this.val.provinceVal = this.addressVal.provinceVal
            this.val.cityVal = this.addressVal.cityVal
            this.val.areaVal = this.addressVal.areaVal
        }
    }
}
</script>

<style>
    html,body{width: 100%; height: 100%;}
    *{padding: 0; margin: 0;}
    .addressSelect{width: 100%; position: relative; background: #fff; height: 140px;overflow: hidden; -webkit-mask-box-image: linear-gradient(0deg,transparent,transparent 5%,#fff 20%,#fff 80%,transparent 95%,transparent);}
    .addressSelect ul{width: 33.333333%; position: absolute; left: 0; top:30px; -webkit-transform-style: preserve-3d; -webkit-backface-visibility:hidden; text-align: center; padding-left: 0;}
    .addressSelect ul li{white-space : nowrap;overflow: hidden; text-overflow:ellipsis; color:rgba(0,0,0,.54); padding: 3px 0;}
    .addressSelect ul:nth-of-type(2){left: 33.333333%;}
    .addressSelect ul:nth-of-type(3){left: 66.666666%;}
    .addressSelect ul li.addSelectActive{color:rgba(0,0,0,.87); transform: scale(1.3); transition: 0.5s;}
    .selectAni{transition: 0.8s;}
    
</style>

