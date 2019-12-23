---
layout: post
title: angularjs 单独组建启动
tags : [angular]
weather: 多云
---



angular生成的项目默认以AppComponent来自动启动项目，通过index.html中嵌入``` <app-root></app-root> ``` 来加载组件。如果只是想作为页面里的自定义tag组件出现，接下来可以用以下步骤：



1. 安装 @angular/elements 

   ```shell
   npm install @angular/elements --save
   ```

2. 修改app.module.ts文件

   ```typescript
   import { BrowserModule } from '@angular/platform-browser';
   import { NgModule ,Injector} from '@angular/core';
   import { createCustomElement } from '@angular/elements';
   import { HttpClientModule } from '@angular/common/http';
   import {HashLocationStrategy, Location, LocationStrategy} from '@angular/common';
   
   import { AppRoutingModule } from './app-routing.module';
   import { AppComponent } from './app.component';
   
   
   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       AppRoutingModule,
       HttpClientModule,
     ],
     providers: [
       Location, {provide: LocationStrategy, useClass: HashLocationStrategy}
     ],
     bootstrap: [], //<--将原来的AppComponent删除
     entryComponents:[
       AppComponent  //<--加入AppComponent
     ]
   })
   
   
   export class AppModule { 
     constructor(private injector: Injector) {
       // 注册自定义的html 标签
       const myElement = createCustomElement(AppComponent, { injector });
       customElements.define('my-tag', myElement);
     }
     ngDoBootstrap() { }
   }
   ```

3. 按原来的方式些组件即可