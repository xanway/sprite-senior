#  Android原生插件开发

----------  

<h2 id="cid_0">概述</h2>  


Android原生插件开发分为UI插件和功能插件，其中功能插件又分为单例插件和多例插件。  

单例插件继承于FHSingletonComponent。  

多例插件继承于FHMultitonComponent。  

UI插件继承于FHUIComponent，其中UI插件分为两个部分，一个是视图控件，继承于FHView，主要用于界面展示和用户交互部分；另外一个是JS对象，继承于FHUIComponent，主要用于js中的处理。  


<h2 id="cid_0">环境搭建</h2>   


1：开发环境：Android studio 2.3.1及以上  

2：EDN论坛下载Sprite[  Android原生开发工程](https://www.exmobi.cn/downloadRedirect.jsp?type=sprite_plugin_android)，该工程与EDN打包工程相同，包含Sprite基础SDK aar包，基础SDK使用的第三方库等  

3：运行该工程，若一切正常则可进入如下图所示Sprite界面  

<img src="image/android_0.png"  width="250"/>


<h2 id="cid_0">插件开发</h2>   

**基础类说明** 

 Sprite已经提供了完善的插件开发及加载相关机制，开发者只需要继承插件基础类，重载关键方法，即可快速实现插件开发。  

其中FHUIComponent，FHMultitonComponent，FHSingletomComponent均继承于FHComponent，组件开发时类相关方法说明如下。

<code>**FHUIView 的方法 ：**</code>  

**init () : void**    <code>初始化函数</code>  

**registerEvent () : void**	 <code>注册触摸事件</code>   

**getSysView () : View**     <code>获取系统View</code>  

**addSubView (FHDomObject child) : void** 	 <code>插入子节点</code>   

参数:   
child -- 待添加子节点dom对象


**removeChild (FHDomObject child) : void** 	 <code>去除子节点</code> 

参数:    
child -- 待删除子节点dom对象  


**updateAttribute () : void ** 	<code>更新属性</code> 


**updateStyle () : void ** 	<code>更新样式</code>   


**textChanged (boolean isInit) : void **	 <code>文本修改后处理</code>  

参数:  
isInit -- 是否在初始化时调用。若设为 false, 表示通过 js 调用，如果样式影响了布局，需要通知布局刷新。  


**attributeChanged (String attrName, String attrValue, boolean isInit) : void** 	 <code>属性修改后处理</code>  

参数:   
attrName -- 属性名  
attrValue -- 属性值  
isInit -- 是否在初始化时调用。若设为 false, 表示通过 js 调用，如果样式影响了布局，需要通知布局刷新。   

**cssChanged (String styleName, String styleValue, boolean isInit) : boolean** 	 <code>样式修改后处理</code>  

参数:   
styleName -- 属性名  
styleValue -- 属性值  
isInit -- 是否在初始化时调用。若设为 false, 表示通过 js 调用，如果样式影响了布局，需要通知布局刷新。    

**setBackgroundColor (int bgColor) : void**	 <code>设置背景颜色</code> 

**updateBorder () : boolean**	 <code>更新边框</code>  

返回值: 是否更新  

**createChildView () : void**  	 <code>遍历创建所有子节点</code>   

**updateChildView () : void**	 <code>遍历更新所有子节点</code>   

**updateChildFrame () : void**	 <code>遍历刷新子节点布局</code>   

**update() : void**	  <code>更新界面，包括样式、属性和文本</code>   

**updateFinish () : void**	 <code>更新样式和属性结束后调用</code>  

**updateFrame () : void**	 <code>刷新布局</code>  

**preferenceChanged () : void** 	 <code>通知刷新CSS Node</code>  

**postInvalidate () : void**	 <code>通知系统调用onDraw方法</code> 

**measure (YogaNodeAPI yogaNodeAPI, float width, YogaMeasureMode widthMeasureMode, float height, YogaMeasureMode heightMeasureMode) : void**	     <code>控件测量函数</code>   

参数:   
yogaNodeAPI -- CSS Node API  
width -- 宽度  
widthMeasureMode -- 宽度模式  
height -- 高度  
heightMeasureMode -- 高度模式    

**setFrame (Size size, Rect rect) : void**	  <code>设置控件Frame</code>  

参数:   
size -- 控件尺寸  
rect -- 控件矩形区域  


**parseStylePath (String oldPath) : String**	 <code>解析样式路径</code> 

**parseAttrPath (String oldPath) : String**	 <code>解析属性路径</code> 

**onResume () : void**	  <code>处理Activity onResume事件(view可见)</code> 

**onChildResume () : void**	  <code>子节点处理Activity onResume事件(view可见)</code> 

**onPause () : void**	 <code>处理Activity onPause 事件(view不可见或者被遮挡)</code> 

**onChildPause () : void**	 <code>子节点处理Activity onPause 事件(view不可见或者被遮挡)</code>   

**destroy () : void**	   <code>销毁对象</code> 


<code>**FHComponent 的方法 ：** </code>

**on (String[] params) : void**	<code>注册事件 </code>

**fire (String[] params) : void**	<code>触发事件</code>  

**off(String[] params) : void**	<code>移除事件 </code> 

**getOn(String[] params) : int[]**  	<code>获取已注册事件函数列表</code>



