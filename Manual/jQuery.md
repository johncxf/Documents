# jQuery学习笔记

### 语法

$(selector).action()

- 美元符号定义 jQuery
- 选择符（selector）“查询”和“查找” HTML 元素
- jQuery 的 action() 执行对元素的操作

### 函数

document ready

```
$(document).ready(function(){

--- jQuery functions go here ----

});
```

### 选择器

#### 元素选择器

jQuery 使用 CSS 选择器来选取 HTML 元素。

$("p") 选取 <p> 元素。

$("p.intro") 选取所有 class="intro" 的 <p> 元素。

$("p#demo") 选取所有 id="demo" 的 <p> 元素。

#### 属性选择器

jQuery 使用 XPath 表达式来选择带有给定属性的元素。

$("[href]") 选取所有带有 href 属性的元素。

$("[href='#']") 选取所有带有 href 值等于 "#" 的元素。

$("[href!='#']") 选取所有带有 href 值不等于 "#" 的元素。

$("[href$='.jpg']") 选取所有 href 值以 ".jpg" 结尾的元素。

#### css选择器

jQuery CSS 选择器可用于改变 HTML 元素的 CSS 属性。

例：

```
$("p").css("background-color","red");
```

#### 参考手册

| 选择器                                                       | 实例                       | 选取                                       |
| ------------------------------------------------------------ | -------------------------- | ------------------------------------------ |
| [*](http://www.w3school.com.cn/jquery/selector_all.asp)      | $("*")                     | 所有元素                                   |
| [#*id*](http://www.w3school.com.cn/jquery/selector_id.asp)   | $("#lastname")             | id="lastname" 的元素                       |
| [.*class*](http://www.w3school.com.cn/jquery/selector_class.asp) | $(".intro")                | 所有 class="intro" 的元素                  |
| [*element*](http://www.w3school.com.cn/jquery/selector_element.asp) | $("p")                     | 所有 <p> 元素                              |
| .*class*.*class*                                             | $(".intro.demo")           | 所有 class="intro" 且 class="demo" 的元素  |
|                                                              |                            |                                            |
| [:first](http://www.w3school.com.cn/jquery/selector_first.asp) | $("p:first")               | 第一个 <p> 元素                            |
| [:last](http://www.w3school.com.cn/jquery/selector_last.asp) | $("p:last")                | 最后一个 <p> 元素                          |
| [:even](http://www.w3school.com.cn/jquery/selector_even.asp) | $("tr:even")               | 所有偶数 <tr> 元素                         |
| [:odd](http://www.w3school.com.cn/jquery/selector_odd.asp)   | $("tr:odd")                | 所有奇数 <tr> 元素                         |
|                                                              |                            |                                            |
| [:eq(*index*)](http://www.w3school.com.cn/jquery/selector_eq.asp) | $("ul li:eq(3)")           | 列表中的第四个元素（index 从 0 开始）      |
| [:gt(*no*)](http://www.w3school.com.cn/jquery/selector_gt.asp) | $("ul li:gt(3)")           | 列出 index 大于 3 的元素                   |
| [:lt(*no*)](http://www.w3school.com.cn/jquery/selector_lt.asp) | $("ul li:lt(3)")           | 列出 index 小于 3 的元素                   |
| :not(*selector*)                                             | $("input:not(:empty)")     | 所有不为空的 input 元素                    |
|                                                              |                            |                                            |
| [:header](http://www.w3school.com.cn/jquery/selector_header.asp) | $(":header")               | 所有标题元素 <h1> - <h6>                   |
| [:animated](http://www.w3school.com.cn/jquery/selector_animated.asp) |                            | 所有动画元素                               |
|                                                              |                            |                                            |
| [:contains(*text*)](http://www.w3school.com.cn/jquery/selector_contains.asp) | $(":contains('W3School')") | 包含指定字符串的所有元素                   |
| [:empty](http://www.w3school.com.cn/jquery/selector_empty.asp) | $(":empty")                | 无子（元素）节点的所有元素                 |
| :hidden                                                      | $("p:hidden")              | 所有隐藏的 <p> 元素                        |
| [:visible](http://www.w3school.com.cn/jquery/selector_visible.asp) | $("table:visible")         | 所有可见的表格                             |
|                                                              |                            |                                            |
| s1,s2,s3                                                     | $("th,td,.intro")          | 所有带有匹配选择的元素                     |
|                                                              |                            |                                            |
| [[*attribute*\]](http://www.w3school.com.cn/jquery/selector_attribute.asp) | $("[href]")                | 所有带有 href 属性的元素                   |
| [[*attribute*=*value*\]](http://www.w3school.com.cn/jquery/selector_attribute_equal_value.asp) | $("[href='#']")            | 所有 href 属性的值等于 "#" 的元素          |
| [[*attribute*!=*value*\]](http://www.w3school.com.cn/jquery/selector_attribute_notequal_value.asp) | $("[href!='#']")           | 所有 href 属性的值不等于 "#" 的元素        |
| [[*attribute*$=*value*\]](http://www.w3school.com.cn/jquery/selector_attribute_end_value.asp) | $("[href$='.jpg']")        | 所有 href 属性的值包含以 ".jpg" 结尾的元素 |
|                                                              |                            |                                            |
| [:input](http://www.w3school.com.cn/jquery/selector_input.asp) | $(":input")                | 所有 <input> 元素                          |
| [:text](http://www.w3school.com.cn/jquery/selector_input_text.asp) | $(":text")                 | 所有 type="text" 的 <input> 元素           |
| [:password](http://www.w3school.com.cn/jquery/selector_input_password.asp) | $(":password")             | 所有 type="password" 的 <input> 元素       |
| [:radio](http://www.w3school.com.cn/jquery/selector_input_radio.asp) | $(":radio")                | 所有 type="radio" 的 <input> 元素          |
| [:checkbox](http://www.w3school.com.cn/jquery/selector_input_checkbox.asp) | $(":checkbox")             | 所有 type="checkbox" 的 <input> 元素       |
| [:submit](http://www.w3school.com.cn/jquery/selector_input_submit.asp) | $(":submit")               | 所有 type="submit" 的 <input> 元素         |
| [:reset](http://www.w3school.com.cn/jquery/selector_input_reset.asp) | $(":reset")                | 所有 type="reset" 的 <input> 元素          |
| [:button](http://www.w3school.com.cn/jquery/selector_input_button.asp) | $(":button")               | 所有 type="button" 的 <input> 元素         |
| [:image](http://www.w3school.com.cn/jquery/selector_input_image.asp) | $(":image")                | 所有 type="image" 的 <input> 元素          |
| [:file](http://www.w3school.com.cn/jquery/selector_input_file.asp) | $(":file")                 | 所有 type="file" 的 <input> 元素           |
|                                                              |                            |                                            |
| [:enabled](http://www.w3school.com.cn/jquery/selector_input_enabled.asp) | $(":enabled")              | 所有激活的 input 元素                      |
| [:disabled](http://www.w3school.com.cn/jquery/selector_input_disabled.asp) | $(":disabled")             | 所有禁用的 input 元素                      |
| [:selected](http://www.w3school.com.cn/jquery/selector_input_selected.asp) | $(":selected")             | 所有被选取的 input 元素                    |
| [:checked](http://www.w3school.com.cn/jquery/selector_input_checked.asp) | $(":checked")              | 所有被选中的 input 元素                    |

### 事件

#### 参考手册

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [bind()](http://www.w3school.com.cn/jquery/event_bind.asp)   | 向匹配元素附加一个或更多事件处理器                           |
| [blur()](http://www.w3school.com.cn/jquery/event_blur.asp)   | 触发、或将函数绑定到指定元素的 blur 事件                     |
| [change()](http://www.w3school.com.cn/jquery/event_change.asp) | 触发、或将函数绑定到指定元素的 change 事件                   |
| [click()](http://www.w3school.com.cn/jquery/event_click.asp) | 触发、或将函数绑定到指定元素的 click 事件                    |
| [dblclick()](http://www.w3school.com.cn/jquery/event_dblclick.asp) | 触发、或将函数绑定到指定元素的 double click 事件             |
| [delegate()](http://www.w3school.com.cn/jquery/event_delegate.asp) | 向匹配元素的当前或未来的子元素附加一个或多个事件处理器       |
| [die()](http://www.w3school.com.cn/jquery/event_die.asp)     | 移除所有通过 live() 函数添加的事件处理程序。                 |
| [error()](http://www.w3school.com.cn/jquery/event_error.asp) | 触发、或将函数绑定到指定元素的 error 事件                    |
| [event.isDefaultPrevented()](http://www.w3school.com.cn/jquery/event_isdefaultprevented.asp) | 返回 event 对象上是否调用了 event.preventDefault()。         |
| [event.pageX](http://www.w3school.com.cn/jquery/event_pagex.asp) | 相对于文档左边缘的鼠标位置。                                 |
| [event.pageY](http://www.w3school.com.cn/jquery/event_pagey.asp) | 相对于文档上边缘的鼠标位置。                                 |
| [event.preventDefault()](http://www.w3school.com.cn/jquery/event_preventdefault.asp) | 阻止事件的默认动作。                                         |
| [event.result](http://www.w3school.com.cn/jquery/event_result.asp) | 包含由被指定事件触发的事件处理器返回的最后一个值。           |
| [event.target](http://www.w3school.com.cn/jquery/event_target.asp) | 触发该事件的 DOM 元素。                                      |
| [event.timeStamp](http://www.w3school.com.cn/jquery/event_timeStamp.asp) | 该属性返回从 1970 年 1 月 1 日到事件发生时的毫秒数。         |
| [event.type](http://www.w3school.com.cn/jquery/event_type.asp) | 描述事件的类型。                                             |
| [event.which](http://www.w3school.com.cn/jquery/event_which.asp) | 指示按了哪个键或按钮。                                       |
| [focus()](http://www.w3school.com.cn/jquery/event_focus.asp) | 触发、或将函数绑定到指定元素的 focus 事件                    |
| [keydown()](http://www.w3school.com.cn/jquery/event_keydown.asp) | 触发、或将函数绑定到指定元素的 key down 事件                 |
| [keypress()](http://www.w3school.com.cn/jquery/event_keypress.asp) | 触发、或将函数绑定到指定元素的 key press 事件                |
| [keyup()](http://www.w3school.com.cn/jquery/event_keyup.asp) | 触发、或将函数绑定到指定元素的 key up 事件                   |
| [live()](http://www.w3school.com.cn/jquery/event_live.asp)   | 为当前或未来的匹配元素添加一个或多个事件处理器               |
| [load()](http://www.w3school.com.cn/jquery/event_load.asp)   | 触发、或将函数绑定到指定元素的 load 事件                     |
| [mousedown()](http://www.w3school.com.cn/jquery/event_mousedown.asp) | 触发、或将函数绑定到指定元素的 mouse down 事件               |
| [mouseenter()](http://www.w3school.com.cn/jquery/event_mouseenter.asp) | 触发、或将函数绑定到指定元素的 mouse enter 事件              |
| [mouseleave()](http://www.w3school.com.cn/jquery/event_mouseleave.asp) | 触发、或将函数绑定到指定元素的 mouse leave 事件              |
| [mousemove()](http://www.w3school.com.cn/jquery/event_mousemove.asp) | 触发、或将函数绑定到指定元素的 mouse move 事件               |
| [mouseout()](http://www.w3school.com.cn/jquery/event_mouseout.asp) | 触发、或将函数绑定到指定元素的 mouse out 事件                |
| [mouseover()](http://www.w3school.com.cn/jquery/event_mouseover.asp) | 触发、或将函数绑定到指定元素的 mouse over 事件               |
| [mouseup()](http://www.w3school.com.cn/jquery/event_mouseup.asp) | 触发、或将函数绑定到指定元素的 mouse up 事件                 |
| [one()](http://www.w3school.com.cn/jquery/event_one.asp)     | 向匹配元素添加事件处理器。每个元素只能触发一次该处理器。     |
| [ready()](http://www.w3school.com.cn/jquery/event_ready.asp) | 文档就绪事件（当 HTML 文档就绪可用时）                       |
| [resize()](http://www.w3school.com.cn/jquery/event_resize.asp) | 触发、或将函数绑定到指定元素的 resize 事件                   |
| [scroll()](http://www.w3school.com.cn/jquery/event_scroll.asp) | 触发、或将函数绑定到指定元素的 scroll 事件                   |
| [select()](http://www.w3school.com.cn/jquery/event_select.asp) | 触发、或将函数绑定到指定元素的 select 事件                   |
| [submit()](http://www.w3school.com.cn/jquery/event_submit.asp) | 触发、或将函数绑定到指定元素的 submit 事件                   |
| [toggle()](http://www.w3school.com.cn/jquery/event_toggle.asp) | 绑定两个或多个事件处理器函数，当发生轮流的 click 事件时执行。 |
| [trigger()](http://www.w3school.com.cn/jquery/event_trigger.asp) | 所有匹配元素的指定事件                                       |
| [triggerHandler()](http://www.w3school.com.cn/jquery/event_triggerhandler.asp) | 第一个被匹配元素的指定事件                                   |
| [unbind()](http://www.w3school.com.cn/jquery/event_unbind.asp) | 从匹配元素移除一个被添加的事件处理器                         |
| [undelegate()](http://www.w3school.com.cn/jquery/event_undelegate.asp) | 从匹配元素移除一个被添加的事件处理器，现在或将来             |
| [unload()](http://www.w3school.com.cn/jquery/event_unload.asp) | 触发、或将函数绑定到指定元素的 unload 事件                   |

### jQuery HTML







#### 获取

三个简单实用的用于 DOM 操作的 jQuery 方法：

- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素的内容（包括 HTML 标记）
- val() - 设置或返回表单字段的值

#### jQuery 文档操作方法手册

| 方法                                                         | 描述                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [addClass()](http://www.w3school.com.cn/jquery/attributes_addclass.asp) | 向匹配的元素添加指定的类名。                           |
| [after()](http://www.w3school.com.cn/jquery/manipulation_after.asp) | 在匹配的元素之后插入内容。                             |
| [append()](http://www.w3school.com.cn/jquery/manipulation_append.asp) | 向匹配元素集合中的每个元素结尾插入由参数指定的内容。   |
| [appendTo()](http://www.w3school.com.cn/jquery/manipulation_appendto.asp) | 向目标结尾插入匹配元素集合中的每个元素。               |
| [attr()](http://www.w3school.com.cn/jquery/attributes_attr.asp) | 设置或返回匹配元素的属性和值。                         |
| [before()](http://www.w3school.com.cn/jquery/manipulation_before.asp) | 在每个匹配的元素之前插入内容。                         |
| [clone()](http://www.w3school.com.cn/jquery/manipulation_clone.asp) | 创建匹配元素集合的副本。                               |
| [detach()](http://www.w3school.com.cn/jquery/manipulation_detach.asp) | 从 DOM 中移除匹配元素集合。                            |
| [empty()](http://www.w3school.com.cn/jquery/manipulation_empty.asp) | 删除匹配的元素集合中所有的子节点。                     |
| [hasClass()](http://www.w3school.com.cn/jquery/attributes_hasclass.asp) | 检查匹配的元素是否拥有指定的类。                       |
| [html()](http://www.w3school.com.cn/jquery/manipulation_html.asp) | 设置或返回匹配的元素集合中的 HTML 内容。               |
| [insertAfter()](http://www.w3school.com.cn/jquery/manipulation_insertafter.asp) | 把匹配的元素插入到另一个指定的元素集合的后面。         |
| [insertBefore()](http://www.w3school.com.cn/jquery/manipulation_insertbefore.asp) | 把匹配的元素插入到另一个指定的元素集合的前面。         |
| [prepend()](http://www.w3school.com.cn/jquery/manipulation_prepend.asp) | 向匹配元素集合中的每个元素开头插入由参数指定的内容。   |
| [prependTo()](http://www.w3school.com.cn/jquery/manipulation_perpendto.asp) | 向目标开头插入匹配元素集合中的每个元素。               |
| [remove()](http://www.w3school.com.cn/jquery/manipulation_remove.asp) | 移除所有匹配的元素。                                   |
| [removeAttr()](http://www.w3school.com.cn/jquery/attributes_removeattr.asp) | 从所有匹配的元素中移除指定的属性。                     |
| [removeClass()](http://www.w3school.com.cn/jquery/attributes_removeclass.asp) | 从所有匹配的元素中删除全部或者指定的类。               |
| [replaceAll()](http://www.w3school.com.cn/jquery/manipulation_replaceall.asp) | 用匹配的元素替换所有匹配到的元素。                     |
| [replaceWith()](http://www.w3school.com.cn/jquery/manipulation_replacewith.asp) | 用新内容替换匹配的元素。                               |
| [text()](http://www.w3school.com.cn/jquery/manipulation_text.asp) | 设置或返回匹配元素的内容。                             |
| [toggleClass()](http://www.w3school.com.cn/jquery/attributes_toggleclass.asp) | 从匹配的元素中添加或删除一个类。                       |
| [unwrap()](http://www.w3school.com.cn/jquery/manipulation_unwrap.asp) | 移除并替换指定元素的父元素。                           |
| [val()](http://www.w3school.com.cn/jquery/attributes_val.asp) | 设置或返回匹配元素的值。                               |
| [wrap()](http://www.w3school.com.cn/jquery/manipulation_wrap.asp) | 把匹配的元素用指定的内容或元素包裹起来。               |
| [wrapAll()](http://www.w3school.com.cn/jquery/manipulation_wrapall.asp) | 把所有匹配的元素用指定的内容或元素包裹起来。           |
| [wrapinner()](http://www.w3school.com.cn/jquery/manipulation_wrapinner.asp) | 将每一个匹配的元素的子内容用指定的内容或元素包裹起来。 |

#### jQuery 属性操作方法手册

| 方法                                                         | 描述                                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| [addClass()](http://www.w3school.com.cn/jquery/attributes_addclass.asp) | 向匹配的元素添加指定的类名。             |
| [attr()](http://www.w3school.com.cn/jquery/attributes_attr.asp) | 设置或返回匹配元素的属性和值。           |
| [hasClass()](http://www.w3school.com.cn/jquery/attributes_hasclass.asp) | 检查匹配的元素是否拥有指定的类。         |
| [html()](http://www.w3school.com.cn/jquery/manipulation_html.asp) | 设置或返回匹配的元素集合中的 HTML 内容。 |
| [removeAttr()](http://www.w3school.com.cn/jquery/attributes_removeattr.asp) | 从所有匹配的元素中移除指定的属性。       |
| [removeClass()](http://www.w3school.com.cn/jquery/attributes_removeclass.asp) | 从所有匹配的元素中删除全部或者指定的类。 |
| [toggleClass()](http://www.w3school.com.cn/jquery/attributes_toggleclass.asp) | 从匹配的元素中添加或删除一个类。         |
| [val()](http://www.w3school.com.cn/jquery/attributes_val.asp) | 设置或返回匹配元素的值。                 |

#### jQuery CSS 操作函数手册

| CSS 属性                                                     | 描述                                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| [css()](http://www.w3school.com.cn/jquery/css_css.asp)       | 设置或返回匹配元素的样式属性。           |
| [height()](http://www.w3school.com.cn/jquery/css_height.asp) | 设置或返回匹配元素的高度。               |
| [offset()](http://www.w3school.com.cn/jquery/css_offset.asp) | 返回第一个匹配元素相对于文档的位置。     |
| [offsetParent()](http://www.w3school.com.cn/jquery/css_offsetparent.asp) | 返回最近的定位祖先元素。                 |
| [position()](http://www.w3school.com.cn/jquery/css_position.asp) | 返回第一个匹配元素相对于父元素的位置。   |
| [scrollLeft()](http://www.w3school.com.cn/jquery/css_scrollleft.asp) | 设置或返回匹配元素相对滚动条左侧的偏移。 |
| [scrollTop()](http://www.w3school.com.cn/jquery/css_scrolltop.asp) | 设置或返回匹配元素相对滚动条顶部的偏移。 |
| [width()](http://www.w3school.com.cn/jquery/css_width.asp)   | 设置或返回匹配元素的宽度。               |