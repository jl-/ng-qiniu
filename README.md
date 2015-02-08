### ng-qiniu
> angular qiniu uploader, with <qiniu-upload> and QiniuService


### Installation
`bower install ng-qiniu --save`

### Usage
```html
<qiniu-upload handle="test-handle" uploader="appCtrl.uploader" opts="appCtrl.uploaderOpts"></qiniu-upload>
<button ng-click="appCtrl.upload()">Upload</button>
<div ng-repeat="file in appCtrl.uploader.files">
    <!--     preview    -->    
    <img-thumb file="file"></img-thumb>
    <button ng-click="appCtrl.removeFile(file)">remove</button>
</div>
```

```javascript
<script src="bower_components/ngQiniu/ng-qiniu.js"></script>
<script>
    var app = angular.module('app',['ngQiniu'])
        .controller('AppCtrl',['$scope',function($scope){
            var scope = this;

            /// oploaderOpts, optional
            scope.oploaderOpts = {
                uptoken_url: '', ///// must set!!
                init: {
                    
                    FilesAdded: function(up, files) {
                        // 文件添加进队列后,处理相关的事情
                     },
                    BeforeUpload: function(up, file) {
                        // 每个文件上传前,处理相关的事情
                    },
                    UploadProgress: function(up, file) {
                        // 每个文件上传时,处理相关的事情
                    },
                    FileUploaded: function(up, file, info) {
                        // 每个文件上传成功后,处理相关的事情
                        // 其中 info 是文件上传成功后，服务端返回的json，形式如
                        // {
                        //    "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
                        //    "key": "gogopher.jpg"
                        //  }
                        // 参考http://developer.qiniu.com/docs/v6/api/overview/up/response/simple-response.html
                        // var domain = up.getOption('domain');
                        // var res = parseJSON(info);
                        // var sourceLink = domain + res.key; 获取上传成功后的文件的Url
                    },
                    Error: function(up, err, errTip) {
                        //上传出错时,处理相关的事情
                    },
                    UploadComplete: function() {
                        //队列文件处理完毕后,处理相关的事情
                    },
                    Key: function(up, file) {
                        // 若想在前端对每个文件的key进行个性化处理，可以配置该函数
                        // 该配置必须要在 unique_names: false , save_key: false 时才生效
                        var key = "";
                        // do something with key here
                        return key;
                    }
                }
            };
            scope.removeFile = function(file){
                scope.uploader.removeFile(file);
            };

            // if scope.uploaderOpts.auto_start !== true
            scope.upload = function(){
                scope.uploader.start();
            };
        }]);
</script>
```