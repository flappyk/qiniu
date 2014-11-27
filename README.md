七牛云储存PHP单文件SDK
=====

适用于IE8+、Chrome、Firefox、Safari 等浏览器，基于 七牛云存储官方API 构建，其中上传功能基于 Plupload 插件封装。开发者使用本 SDK 可以方便的从浏览器端上传文件至七牛云存储，并对上传成功后的图片进行丰富的数据处理操作。本 SDK 可使开发者忽略上传底层实现细节，而更多的关注 UI 层的展现。


功能简介
===

上传
===

html5模式大于4M时可分块上传，小于4M时直传
Flash、html4模式直接上传
继承了Plupload的功能，可筛选文件上传、拖曳上传等

服务端准备
===

本SDK依赖服务端颁发upToken，可以通过以下二种方式实现：
利用服务端SDK构建后端服务
后端服务应提供一个URL地址，供SDK初始化使用，前端通过Ajax请求该地址后获得upToken。Ajax请求成功后，服务端应返回如下格式的json：

    {
        "uptoken": "0MLvWPnyya1WtPnXFy9KLyGHyFPNdZceomL..."
    }

引入基础库 qiniu.zip.min.js
引入上传插件 jquyer.upload.js

初始化上传
<div id="container">
    <button data-name="file">上传文件</button>
    <input name="file"/>
</div>
<script>
    $(function () {
        $('button').upload();
    });
</script>

更新配置参考 http://plupload.com/docs/
