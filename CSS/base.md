<!-- 

        animation: 
            [
                <animation-name> : 动画名称(@keyframes animationName)
                <animation-duration> ||
                <animation-timing-function> || 
                <animation-delay> || 
                <animation-iteration-count> : [ infinite | <number> ] [, [ infinite | <number> ] ] ----- (initial | inherit | unset)
                <animation-direction> : [ normal | reverse | alternate | alternate-reverse ] [, [ normal | reverse | alternate | alternate-reverse ] ] ----- (initial | inherit | unset)
                <animation-fill-mode> : 可让您定义动画在执行时间之外应用的值。可以在应用动画之前或在完成运行之后。[ none | forwards | backwards | both ] [, [ none | forwards | backwards | both ] ]*
                animation-play-state: [ running | paused ] [, [ running | paused ] ]* (An animation that is running can be paused by setting the animation-play-state property to paused. To continue running the animation, the property can be set to running.   A paused animation will continue to display the current value of the animation in a static state, as if the time of the animation is constant. When a paused animation is resumed, it restarts from the current value, not necessarily from the beginning of the animation.)
            ] 
            [, 
                [
                    <animation-name> || 
                    <animation-duration> || 
                    <animation-timing-function> || 
                    <animation-delay> || 
                    <animation-iteration-count> || 
                    <animation-direction> || 
                    <animation-fill-mode>
                ] 
            ]
        transform: none | <transform-function> [ <transform-function> ]*
            2D tranform functions:
                matrix() = matrix( <number> [,<number> ]{5,5} )  : This function specifies a 2D transformation in the form of a transformation matrix of the six values a-f (that is, a, b, c, d, e and f).
                translateX( <translation-value> )
                translateY( <translation-value> )
                scale( <number>[, <number>]? )
                scaleX( <number>[, <number>]? )
                scaleY( <number>[, <number>]? )
                rotate( <angle> )
                skew( <angle> [, <angle> ]? )
                skewX( <angle> )
                skewY( <angle> )
            3D tranform functions:
                matrix3d() = matrix3d( <number> [, <number> ]{15,15} ) : This function specifies a 3D transformation as a 4x4 homogeneous matrix of 16 values in column-major order.
                translate3d() = translate3d( <translation-value> , <translation-value> , <length> )
                translateZ() = translateZ( <length> )
                scale3d() = scale3d( <number> , <number> , <number> )
                scaleZ() = scaleZ( <number> ) ：This function specifies a 3D scale operation using the [1,1,sz] scaling vector, where sz is given as the parameter.
                rotate3d() = rotate3d( <number> , <number> , <number> , <number> )
                perspective() = perspective( <length> )：This function defines the distance between the z=0 plane and the user in order to give to the 3D-positioned element some perspective.

        transform-origin: <x-axis> <y-axis> <z-axis>
            Possible Values：
            [ <percentage> | <length> | left | center | right | top | bottom ] |
            [
                [ <percentage> | <length> | left | center | right ]
                &&
                [ <percentage> | <length> | top | center | bottom ] 
            ] <length>?


            x-axis: <percentage> | <length> | left | center | right 
            y-axis: <percentage> | <length> | top | bottom  
            z-axis: <length>

            In addition, all CSS properties also accept the following CSS-wide keyword values as the sole component of their property value:

            initial | inherit | unset   



            cubic-bezier_function: https://www.quackit.com/css/functions/css_cubic-bezier_function.cfm
            A cubic Bézier curve is defined by four control points, P0, P1, P2, and P3 as shown in the following diagram.
            The P0 and P3 control points are always set to (0,0) and (1,1). In other words, they don't move.
            However, P1 and P2 can be moved with the cubic-bezier() function. 
            You can specify the location of these two control points by using an x and y value. Like this: x1, y1, x2, y2.
            ease - Equivalent to cubic-bezier(0.25, 0.1, 0.25, 1).
            linear - This function is equivalent to cubic-bezier(0, 0, 1, 1).
            ease-in - This function is equivalent to cubic-bezier(0.42, 0, 1, 1).
            ease-out - This function is equivalent to cubic-bezier(0, 0, 0.58, 1).
            ease-in-out - This function is equivalent to cubic-bezier(0.42, 0, 0.58, 1).s

-->

<!-- 

    Multi-Column:(兼容性不好)
        column-rule: <'column-rule-width'> || <'column-rule-style'> || [ <'column-rule-color'> | transparent ]
            column-rule-style: none, hidden, dotted, dashed, solid, double, groove, ridge, inset, and outset

-->


### html5基础知识、核心技术与前沿案例
常见的非内容标签包括<meta>、<img>、<br>、<hr>、<input>、<link>、<embed>等
#### html标签属性
lang
```html
<html lang="en">
<html lang="fr">
<html lang="zh-CN">
<html manifest="cache.manifest">
```
**注意：**上述en、fr等均为语言代码。目前，lang属性语言代码的国际标准是IETF的BCP 47（参见http://tools.ietf.org/html/bcp47）。在该标准中，简体中文更标准的写法应为zh-cmn-Hans。     
Manifest主要适用于不依赖网络，且下载后不需要再次更新的HTML5游戏、应用或页面，在需要频繁或偶尔更新内容的页面中应慎用manifest。总体来说，其应用面目前还较窄。manifest文件格式以及试用方式参考本书。    
