xlrswiper
====
简易轮播图vue插件
-------
* 作者邮箱：2643671876@qq.com
* github地址：https://github.com/xuelerong/xlr-swiper

安装
-------
    npm install xlrswiper
    
快速上手
-------
    import Vue from "vue"
    import {xlrswiper,xlrswiperitem} from 'xlrswiper'
    import "xlrswiper/style/xlr-swiper.css"
    
    Vue.use(xlrswiper)
    Vue.use(xlrswiperitem)
    
使用组件
-------
    <xlr-swiper :banner="banners">
            <xlr-swiper-item v-for="(item,index) in banners" :key="index">
                <img :src="item.image" alt="">
            </xlr-swiper-item>
    </xlr-swiper>
    
参数说明
-------
    字段名            字段类型       默认值       说明
    autoSwiper       Boolean        false       开启自动轮播
    timeout          Number         3000        轮播时间间隔
    
    

   

