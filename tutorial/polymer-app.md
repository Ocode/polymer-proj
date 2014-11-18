
Polymer APP
===========

## 步骤 1: 搭建 app 的结构


#### 步骤1：代码

默认页面
    
    <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
    <script src="../bower_components/platform/platform.js">
    
    <body unresolved touch-action="auto">
    
之后追加 HTML import 连接将 <core-header-panel>, <core-toolbar>, 和 <paper-tabs> elements 导入:

    <link rel="import" href="../bower_components/core-header-panel/core-header-panel.html">
    <link rel="import" href="../bower_components/core-toolbar/core-toolbar.html">
    <link rel="import" href="../bower_components/paper-tabs/paper-tabs.html">
   
使用组件
   
     <core-header-panel>

      <core-toolbar>
      </core-toolbar>

      <!-- main page content will go here --> 

    </core-header-panel>
   
     
添加tabs （core-toolbar里）
    
    <paper-tabs id="tabs" selected="all" self-end>
      <paper-tab name="all">ALL</paper-tab>
      <paper-tab name="favorites">FAVORITES</paper-tab>
    </paper-tabs>
    
添加样式：

    core-header-panel {
      height: 100%;
      overflow: auto;
      -webkit-overflow-scrolling: touch; 
    }
    core-toolbar {
      background: #03a9f4;
      color: white;
    }
    #tabs {
      width: 100%;
      margin: 0;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
    }

追加一个sctipt监听tabs切换

  <script>
    var tabs = document.querySelector('paper-tabs');

    tabs.addEventListener('core-select', function() {
      console.log("Selected: " + tabs.selected);
    });
  </script>




## 步骤 2: 使用我们自己的 element

    <post-card>
      <img src="profile-picture.png">
      <h2>A. Developer</h2>
      <p>Something really profound about code.</p>
    </post-card>

#### 自定义元素 post-cart.html

首先引进依赖等等


新增结构

    <div class="card-header" layout horizontal center>
      <content select="img"></content>
      <content select="h2"></content>
    </div>
    <content></content>

样式

    polyfill-next-selector { content: '.card-header h2'; }
    .card-header ::content h2 {
      margin: 0;
      font-size: 1.8rem;
      font-weight: 300;
    }
    polyfill-next-selector { content: '.card-header img'; }
    .card-header ::content img {
      width: 70px;
      border-radius: 50%;
      margin: 10px;
    }


index.html中导入post-card.html

    <link rel="import" href="post-card.html">

添加一个 <post-card> element 到index.html 里，直接追加在 <core-toolbar> element 的后面:

    <div class="container" layout vertical center>
      <post-card>
        <img width="70" height="70" 
          src="../images/avatar-07.svg">
        <h2>Another Developer</h2>
        <p>I'm composing with shadow DOM!</p>
      </post-card>
    </div>
    
    
    
## 步骤 3: 使用数据绑定
    
    {
      "uid": 2,
      "text" : "Loving this Polymer thing.",
      "username" : "Rob",
      "avatar" : "../images/avatar-02.svg",
      "favorite": false
    }
    
编辑 post-list.html

追加一个 <post-service> 到 <template> 里:
    
    <post-service id="service" posts="{{posts}}">
    </post-service>

动态渲染一个卡片列表
 
    <div layout vertical center>
      <template repeat="{{post in posts}}">
        <post-card>
          <img src="{{post.avatar}}" width="70" height="70">
          <h2>{{post.username}}</h2>
          <p>{{post.text}}</p>
        </post-card>
      </template>
    </div>


index.html 导入 post-list.html，并使用 post-list 元素

    <post-list show="all"></post-list>
    
    


##  步骤 4: 画龙点睛

追加心型按钮，编辑post-card.html

    <core-icon-button
      id="favicon"
      icon="favorite"
      on-tap="{{favoriteTapped}}">
    </core-icon-button>


    Polymer({
      publish: {
        favorite: {
          value: false,
          reflect: true
        }
      },
      favoriteTapped: function(event, detail, sender) {
        this.favorite = !this.favorite;
        this.fire('favorite-tap');
      }
    });



    core-icon-button {
      position: absolute;
      top: 3px;
      right: 3px;
      fill: #636363;
    }
    :host([favorite]) core-icon-button {
      fill: #da4336;
    }


首页

    var tabs = document.querySelector('paper-tabs');
    var list = document.querySelector('post-list');

    tabs.addEventListener('core-select', function() {
      list.show = tabs.selected;
    });


编辑 post-list.html


    <template repeat="{{post in posts}}">
      
      <post-card
        favorite="{{post.favorite}}"
        on-favorite-tap="{{handleFavorite}}"
        hidden?="{{show == 'favorites' && !post.favorite}}">
        
        <img src="{{post.avatar}}" width="70" height="70">
        <h2>{{post.username}}</h2>
        <p>{{post.text}}</p>
      </post-card>
    </template>

    Polymer({
      handleFavorite: function(event, detail, sender) {
        var post = sender.templateInstance.model.post;
        this.$.service.setFavorite(post.uid, post.favorite);
      }
    });






齐活，收工



