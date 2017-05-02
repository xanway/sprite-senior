#  Sprite自定义组件开发

----------  

<h2 id="cid_0">Sprite组件开发注意事项</h2>  

开发Sprite组件之前，务必先了解这几个注意事项，避免走弯路。

1.	由于组件名和组件文件名一致，所以同一个应用中模板组件名必须唯一，而且路径也必须唯一，比如buttom.component模板文件，对应的组件标签是button，那么在整个应用中就只能有一个button组件的文件，不同路径下也不能有相同的组件名称，如果有其他相同组件需要另起名字比如buttonFH。

2.	在模板内部构建模板布局的时候不能在created方法里面执行document.refresh()布局刷新操作，如非要执行可以使用定时，因为模板构建的时候整个界面都在布局中，如果执行了document.refresh() 可能会影响其他控件布局展示。

3.	在模板内部使用相对路径时，其路径是基于模板文件的相对路径。这样有利于模板资源的封装。

4.	在模板内部动态创建标签时如果想受到模板内部样式的影响 在创建控件的方法里面需要添加第二个参数this。

5.	模板外部的样式都会自动压入模板内部的第一个布局标签上面，比如button组件的样式会自动作用到button.component里面的最外层的box上，属性也是一样但除了id,style,class这三个固定属性。

6.	模板内部最外层布局标签在view层上等同于外部组件本身，但是属于不同的dom对象，如果在模板内部对外层布局标签做事件监听操作，同时外部组件有同样的事件，都会监听到。

7.	模板和外部uixml属于同一个js环境，如果在uixml里面传入一个变量到模板内部，在模板内部修改后，外部变量也会受到影响。

8.	模板和模板之间可以嵌套使用，比如在一个模板里面引入了另外一个模板。

9.	模板内部组件相对外部uixml是封闭的，如果想在uixml里面得到模板内部控件或者某个对象，需要模板开发者暴露方法。

10.	在封装模板组件的时候定义方法和事件的时候，避免使用sprite已有的方法和事件名称，比如refresh方法，sprite平台已经有这个方法，如果自己定义成相同的方法名，可能会引起冲突。

11.	由于模板使用基础控件进行封装，除自定义属性、样式、方法、事件外，模板组件具备基础组件的所有属性、样式、方法和事件。如模板组件最外层用box封装，那么模板组件本身就具备box的所有属性、样式、方法和事件。只不过模板组件封装后有特定的使用场景，有些属性、样式、方法和事件可能不太适合直接用，开发者需要在组件文档中做特殊说明。

12. Sprite组件机制中，css样式有2层，一层是外部组件的css样式，另外一层是组件内部根节点的css样式，由于模板组件具有封装的特点，所以外层样式的优先级高于内部根节点的样式。


<h2 id="cid_0">Sprite组件开发步骤</h2>  

**第一步，新建模板文件xx.component**

在新建模板组件文件前，先创建一个模板组件目录，该目录用来存放组件文件、组件样式还有一些图片资源。

新建模板文件时，模板组件的名称必须和模板文件名保持一致，比如我们想做一个button组件，那么文件名就必须是button.component。另外整个应用全局组件名称不能相同。

模板文件框架内容如下：

```html
<page>
  <module>
    <![CDATA[
     <!--这里可以引用其他模板组件文件-->

    ]]>
  </module>
  <script>
    <![CDATA[

    //定义一个模板类
    var Component = function () {
      //这里定义一些变量

    };

    Component.prototype = {
      //模板组件创建的时候执行
      created: function () { },
      //属性变更回调函数
      attrChanged: function (attrName, attrValue) { },
      //样式变更回调函数
      styleChanged: function (styleName, styleValue) { },
      //监听横竖屏切换时候的回调
      orientationChanged: function (orientation, screenWidth, screenHeight) { }

    }

    module.exports = Component;
    ]]>
  </script>
  <style>

  </style>
  <ui>
    <box>

    </box>
  </ui>
</page>
```

上述代码中是模板组件的基本结构，其中：

> module节点里面可以通过requrie("模板地址")来引用其他模板组件；
> 
> script节点用来写模板的js；
> 
> style节点用来写模板的样式；
> 
> ui节点用来写模板的布局


模板组件里面有几个固定回调函数：

created：模板组件创建的时候会执行，只要在uixml布局里面写模板标签，该函数就会执行。

attrChanged：一般用于外部对组件模板进行属性动态修改时，当模板组件的属性发生变化时进入该回调函数。如果属性直接写在组件标签上面，该函数不会执行。

styleChanged：一般用于外部对组件模板样式进行动态修改时，会进入该回调函数。如果样式直接写在组件标签上面，该函数不会执行。

orientationChanged：函数为页面横竖屏切换回调函数,该函数用于页面横竖屏时通知组件内部处理相关逻辑，回调函数参数：
> orientation：当前屏幕方向,字符串枚举类型,【portrait, landscape】,portrait:竖屏,landscape:横屏
> screenWidth：当前窗口绘制区域宽度；
> screenHeight：当前窗口绘制区域高度


模板组件对外接口主要通过&lt;script&gt;节点中定义的module.exports对象来标识，这个千万不能少。


**第二步：设计组件**


**第三步：组件样式布局**

模板组件内部必须要有一个根节点，一般是box标签，然后按照设计进行界面布局，模板组件的样式可以写在&lt;style&gt;节点里面，比如我们要做一个button组件，可以先做好布局如下：

```html
<box id="btn" class="button">
            <image id="btn_image" class="button-image" style="display:none" />
            <box id="btn_content" class="row-flex-center">
                <image id="loadingimage" style="width:15;height:15;display:none" class="button-image" src="loading.gif" />
                <text id="text" class="button-text"></text>
            </box>
            <box id="tipbox" class="tip" style="display:none">
                <text id="tiptext" class="tip-text">9</text>
            </box>
            <image id="btn_rimage" class="button-image" style="display:none" />
</box>
```

样式可以提取到css文件中进行引用。

```html
 <style>     
        @import url("button.layout.css");
        @import url("button.color.css");
</style>
```



如果马上要在页面看效果可以直接在uixml页面写&lt;button /&gt;，



**第四步：组件js逻辑完善**