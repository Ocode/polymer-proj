Topeka练习
==========

### Direct Git:

This is the best way to not be broken. The Polymer team doesn't use bower in day-to-day development and so Topeka might be broken more frequently if you use the Bower-based workflow.

1. Checkout topeka
2. Do the following:

        mkdir components
        cd components
        git clone https://github.com/Polymer/tools.git
        cd ..
        ./components/tools/bin/pull-all.sh #get a copy of all of Polymer

### Bower

  1. Checkout topeka
  1. Make sure you have `npm` and `bower` [installed](http://bower.io/) installed.
  1. `cd topeka` and `bower install`


### 这里如此规划

除core和paper的elements组件外，其他的elements元素，全部放置在本项目内的elements文件夹内

    分类说明
    <!--system components-->
    core-elements
    paper-elements
    more-elements

    <!--user components-->
    custom-elements
    topeka-elements
    ...


