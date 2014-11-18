
Polymer APP
===========

## ���� 1: � app �Ľṹ


#### ����1������

Ĭ��ҳ��
    
    <meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0, user-scalable=yes">
    <script src="../bower_components/platform/platform.js">
    
    <body unresolved touch-action="auto">
    
֮��׷�� HTML import ���ӽ� <core-header-panel>, <core-toolbar>, �� <paper-tabs> elements ����:

    <link rel="import" href="../bower_components/core-header-panel/core-header-panel.html">
    <link rel="import" href="../bower_components/core-toolbar/core-toolbar.html">
    <link rel="import" href="../bower_components/paper-tabs/paper-tabs.html">
   
ʹ�����
   
     <core-header-panel>

      <core-toolbar>
      </core-toolbar>

      <!-- main page content will go here --> 

    </core-header-panel>
   
     
���tabs ��core-toolbar�
    
    <paper-tabs id="tabs" selected="all" self-end>
      <paper-tab name="all">ALL</paper-tab>
      <paper-tab name="favorites">FAVORITES</paper-tab>
    </paper-tabs>
    
�����ʽ��

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

׷��һ��sctipt����tabs�л�

  <script>
    var tabs = document.querySelector('paper-tabs');

    tabs.addEventListener('core-select', function() {
      console.log("Selected: " + tabs.selected);
    });
  </script>




## ���� 2: ʹ�������Լ��� element

    <post-card>
      <img src="profile-picture.png">
      <h2>A. Developer</h2>
      <p>Something really profound about code.</p>
    </post-card>

#### �Զ���Ԫ�� post-cart.html

�������������ȵ�


�����ṹ

    <div class="card-header" layout horizontal center>
      <content select="img"></content>
      <content select="h2"></content>
    </div>
    <content></content>

��ʽ

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


index.html�е���post-card.html

    <link rel="import" href="post-card.html">

���һ�� <post-card> element ��index.html �ֱ��׷���� <core-toolbar> element �ĺ���:

    <div class="container" layout vertical center>
      <post-card>
        <img width="70" height="70" 
          src="../images/avatar-07.svg">
        <h2>Another Developer</h2>
        <p>I'm composing with shadow DOM!</p>
      </post-card>
    </div>
    
    
    
## ���� 3: ʹ�����ݰ�
    
    {
      "uid": 2,
      "text" : "Loving this Polymer thing.",
      "username" : "Rob",
      "avatar" : "../images/avatar-02.svg",
      "favorite": false
    }
    
�༭ post-list.html

׷��һ�� <post-service> �� <template> ��:
    
    <post-service id="service" posts="{{posts}}">
    </post-service>

��̬��Ⱦһ����Ƭ�б�
 
    <div layout vertical center>
      <template repeat="{{post in posts}}">
        <post-card>
          <img src="{{post.avatar}}" width="70" height="70">
          <h2>{{post.username}}</h2>
          <p>{{post.text}}</p>
        </post-card>
      </template>
    </div>


index.html ���� post-list.html����ʹ�� post-list Ԫ��

    <post-list show="all"></post-list>
    
    


##  ���� 4: �����㾦

׷�����Ͱ�ť���༭post-card.html

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


��ҳ

    var tabs = document.querySelector('paper-tabs');
    var list = document.querySelector('post-list');

    tabs.addEventListener('core-select', function() {
      list.show = tabs.selected;
    });


�༭ post-list.html


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






���չ�



