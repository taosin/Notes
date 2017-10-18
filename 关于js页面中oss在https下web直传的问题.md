# 关于js页面中oss在https下web直传的问题
在页面中进行web端直传，开发环境下测试没有问题，但部署到服务器上时出现了问题，服务器上的站点是基于https协议进行访问的，在此情况下，web端直传出现了异常，原因是在HTTPS的网页中，不允许发起HTTP的请求，可以使用https的endpoint，示例如下：

```javascript
var client = new OSS.Wrapper({ 
  region: 'oss-cn-shanghai', 
  secure: true,     //为true时，即允许发起http请求
  accessKeyId: '', 
  accessKeySecret: '' 
}); 
```