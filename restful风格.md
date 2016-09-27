##restful风格url

前面说的那种URL风格并不是表示那就是restful。

ResultFul推荐每个URL能操作具体的资源,而且能准确描述服务器对资源的处理动作，通常服务器对资源支持get/post/put/delete/等，用来实现资源的增删改查，但是通常用浏览器访问资源都是GET，增加都是POST，而修改和删除不能正确描述。

比如xxxxx/user/1，我既要能表示我要找id为1的user，我还要能表示我能删掉id为1的user；

xxxxx/user,我既要能表示添加一个user，又要能表示修改一个user；

问题是现在的浏览器只支持post/get，它根本无法让服务器知道，我到底要查找user还是要删除User,要添加还是要修改user。

所以第三方框架为了实现这种效果而做了特定的规则去模拟实现，比如spring就用了

<filter> 
  <filter-name>HiddenHttpMethodFilter</filter-name> 
  <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class> 
</filter> 
这么个filter来模拟实现，以支持更多的http操作(put,delete)，当我需要修改一个user的时候，只需要在<form>中加入<input type="hidden" name="_method" value="PUT">,这样提交这个表单的时候spring会知道这是要修改一个user。

具体的不想打字了，这种url风格使得项目架构清晰，好处一时说不上来，但是习惯性的使用这种风格，确实很方便。