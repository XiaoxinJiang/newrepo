# 事件代理
### 什么是事件代理？
又为事件委托，是把原本要绑定的事件委托给父元素，让父元素担当事件监听。  
原理是什么？
原理是dom元素的冒泡机制。
示例
// 获取父节点，并为它添加一个click事件（创建一个id为parent-list的ul）      
document.getElementById("parent-list").addEventListener("click",function(e) {   
// 检查事件源e.targe是否为Li   
  if(e.target && e.target.nodeName.toUpperCase == "li") {  
    // 真正的处理过程在这里   
    console.log("List item ",e.target.id.replace("post-")," was clicked!");
  }
});

jq中的事件委托处理（on和delegate）
$("#link-list").delegate("a", "click", function(){
  // "$(this)" is the node that was clicked
  console.log("you clicked a link!",$(this));
});
jQuery的delegate的方法需要三个参数，一个选择器，一个时间名称，和事件处理函数。

事件冒泡及捕获
DOM2.0模型将事件处理流程分为三个阶段：一、事件捕获阶段，二、事件目标阶段，三、事件起泡阶段。

事件捕获：当某个元素触发某个事件（如onclick），顶层对象document就会发出一个事件流，随着DOM树的节点向目标元素节点流去，直到到达事件真正发生的目标元素。在这个过程中，事件相应的监听函数是不会被触发的。
事件目标：当到达目标元素之后，执行目标元素该事件相应的处理函数。如果没有绑定监听函数，那就不执行。
事件起泡：从目标元素开始，往顶层元素传播。途中如果有节点绑定了相应的事件处理函数，这些函数都会被一次触发。如果想阻止事件起泡，可以使用e.stopPropagation()（Firefox）或者e.cancelBubble=true（IE）来组织事件的冒泡传播。

优点
1.管理的函数变少了。不需要为每个元素都添加监听函数。对于同一个父节点下面类似的子元素，可以通过委托给父元素的监听函数来处理事件。
2.可以方便地动态添加和修改元素，不需要因为元素的改动而修改事件绑定。
3.JavaScript和DOM节点之间的关联变少了，这样也就减少了因循环引用而带来的内存泄漏发生的概率。
