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

upload参数对象
{
  // 是否启用多文件上传
  multi_selection: true, 
  
  // 上传模式,依次退化
  runtimes: 'html5,flash,html4',
  
  // 上传选择的点选按钮，**必需**
  browse_button: option.id,
  // Ajax请求upToken的Url，**强烈建议设置**（服务端提供）
  uptoken_url: 'uptoken.php',
  // 默认 false，key为文件名。若开启该选项，SDK会为每个文件自动生成key（文件名）
  unique_names: false,
  // 默认 false。若在服务端生成uptoken的上传策略中指定`sava_key`，则开启，SDK在前端将不对key进行任何处理
  save_key: false,
  // bucket 域名，下载资源时用到，**必需**
  domain: 'http://wxshop.qiniudn.com/',
  // 上传区域DOM ID，默认是browser_button的父元素，
  container: 'container',
  // 最大文件体积限制
  max_file_size: '500mb',
  // 引入flash,相对路径
  flash_swf_url: 'Moxie.swf', 
  // 上传失败最大重试次数
  max_retries: 3,
  // 开启可拖曳上传
  dragdrop: true, 
  // 拖曳上传区域元素的ID，拖曳文件或文件夹后可触发上传
  drop_element: 'container', 
  // 分块上传时，每片的体积
  chunk_size: '4mb', 
  // 选择文件后自动上传，若关闭需要自己绑定事件触发上传,
  auto_start: true,
  // 上传处理函数
  init:{
    FilesAdded: function (up, files) {
        plupload.each(files, function (file) {
            /* 文件添加进队列后,处理相关的事情 */
        });
    },
    BeforeUpload: function (up, file) {
        /* 每个文件上传前,处理相关的事情 */
    },
    UploadProgress: function (up, file) {
        var msg = file.percent + '%';
        jQuery('[name="' + option.name + '"]').val(msg);
        /* 每个文件上传时,处理相关的事情 */
    },
    FileUploaded: function (up, file, info) {
        /* 每个文件上传成功后,处理相关的事情
         * 其中 info 是文件上传成功后，服务端返回的json，形式如
         *  {
         *    "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
         *    "key": "gogopher.jpg"
         *  }
         */
        var domain = up.getOption('domain');
        var res = jQuery.parseJSON(info);
        var sourceLink = domain + res.key; //获取上传成功后的文件的Url
        jQuery('[name="' + option.name + '"]').val(sourceLink);
    },
    Error: function (up, err, errTip) {
        console.dir(arguments);
        /* 上传出错时,处理相关的事情 */
    },
    UploadComplete: function () {
        console.dir(arguments);
        /* 队列文件处理完毕后,处理相关的事情 */
    },
    Key: function (up, file) {
        /* 若想在前端对每个文件的key进行个性化处理，可以配置该函数
         * 该配置必须要在 unique_names: false , save_key: false 时才生效
         */
        var d = new Date();
        var key = d.getFullYear() + '';
        key += (d.getMonth() + 1);
        key += d.getDay();
        key += d.getHours();
        key += d.getMinutes();
        key += d.getSeconds();
        key += '/';
        key += file.name;
        return key;
    }
  }
}

更新配置参考 http://plupload.com/docs/
