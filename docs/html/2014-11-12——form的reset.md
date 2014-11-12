#form的reset

今天在看JC.FormLogic的源码，对reset重新认识一下，下面转载一篇脚本之家的文章。

HTML中Form表单的reset方法被用来清空用户所输入的内容，以前一直误以为其是单纯的将input等输入项中的值清空。 

但实际上不是这样的，reset方法的本质是将input等输入项中的内容还原为属性value中的值，而不是“”空值。 

w3c上是这样说的： 

在 HTML 表单中 <input type="reset"> 标签每出现一次，一个 Reset 对象就会被创建。 

当重置按钮被点击，包含它的表单中所有输入元素的值都重置为它们的默认值。默认值由 HTML value 属性或 JavaScript 的 defaultValue 属性指定。 

在实际情况中，我们经常需要在编辑某个内容的时候实现表单reset，但是这个时候input等输入项的value属性可能已经被赋予了值，所以reset只是让表单初始化为这个值。 

在这种情况下，我们只能通过javascript去将input等输入项的value属性设置为空来达到reset的效果。