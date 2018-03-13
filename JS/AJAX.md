####基本步骤

AJAX包括以下几个步骤。

1.  创建AJAX对象
2.  发出HTTP请求
3.  接收服务器传回的数据
4.  更新网页数据

即AJAX通过原生的`XMLHttpRequest`对象发出HTTP请求，得到服务器返回的数据后，再进行处理。

AJAX一般是异步请求，同步请求会产生堵塞效应。即请求的文件还未下载完成，后面的同步操作无法执行。

### XMLHttpRequest对象

`XMLHttpRequest`对象用来在浏览器与服务器之间传送数据。XMLHttpRequest可以报送各种数据，包括字符串和二进制，而且除了HTTP，它还支持通过其他协议传送（比如File和FTP）。

```js
//经典用法
var xhr = new XMLHttpRequest();


xhr.onreadystatechange = function(){// 指定回调函数，监听通信状态（readyState属性）的变化。
  if (xhr.readyState === 4){ // 通信成功时，状态值为4
    if (xhr.status === 200){
      console.log(xhr.responseText);
    } else {
      console.error(xhr.statusText);
    }
  }
};

xhr.onerror = function (e) {
  console.error(xhr.statusText);
};

// open方式用于指定HTTP动词、请求的网址、true表示异步请求，false表示同步
xhr.open('GET', '/endpoint', true);

// 发送HTTP请求
xhr.send(null);
```

### XMLHttpRequest实例的属性

#### readyState

`readyState`是一个==只读==属性，用一个整数和对应的常量，表示XMLHttpRequest请求当前所处的状态。

-   0，对应常量`UNSENT`，表示XMLHttpRequest实例已经生成，但是`open()`方法还没有被调用。
-   1，对应常量`OPENED`，表示`send()`方法还没有被调用，仍然可以使用`setRequestHeader()`，设定HTTP请求的头信息。
-   2，对应常量`HEADERS_RECEIVED`，表示`send()`方法已经执行，并且头信息和状态码已经收到。
-   3，对应常量`LOADING`，表示正在接收服务器传来的body部分的数据，如果`responseType`属性是`text`或者空字符串，`responseText`就会包含已经收到的部分信息。
-   4，对应常量`DONE`，表示服务器数据已经完全接收，或者本次接收已经失败了

每当发生状态变化的时候，`readyState`属性的值就会发生改变。这个值每一次变化，都会触发`readyStateChange`事件。

#### onreadystatechange

`onreadystatechange`属性指向一个回调函数，当`readystatechange`事件发生的时候，这个回调函数就会调用，并且XMLHttpRequest实例的`readyState`属性也会发生变化。

另外，如果使用`abort()`方法，终止XMLHttpRequest请求，`onreadystatechange`回调函数也会被调用。

#### responseType

`responseType`属性用来指定服务器返回数据（`xhr.response`）的类型。

-   ”“：字符串（默认值）
-   “arraybuffer”：ArrayBuffer对象
-   “blob”：Blob对象
-   “document”：Document对象
-   “json”：JSON对象
-   “text”：字符串

```js
var xhr = new XMLHttpRequest();
xhr.open('GET', '/path/to/image.png', true);
xhr.responseType = 'json';//在支持JSON的浏览器里，会自动调用JSON.parse()，返回一个JSON对象
```

#### response

`response`属性为==只读==，返回接收到的**数据体**（即body部分）。它的类型可以是ArrayBuffer、Blob、Document、JSON对象、或者一个字符串，这由`XMLHttpRequest.responseType`属性的值决定。

如果本次请求没有成功或者数据不完整，该属性就会等于`null`。

#### responseText

`responseText`属性返回从服务器接收到的**字符串**，该属性为==只读==。如果本次请求没有成功或者数据不完整，该属性就会等于`null`。如果服务器返回的数据格式是JSON，就可以使用`responseText`属性。

#### responseXML

`responseXML`属性返回从服务器接收到的**Document对象**，该属性为 ==只读==。如果本次请求没有成功，或者数据不完整，或者不能被解析为XML或HTML，该属性等于`null`。返回的数据会被直接解析为DOM对象。

#### status

`status`属性为只读属性，表示本次请求所得到的HTTP状态码

-   200, OK，访问正常
-   301, Moved Permanently，永久移动
-   302, Move temporarily，暂时移动
-   304, Not Modified，未修改
-   307, Temporary Redirect，暂时重定向
-   401, Unauthorized，未授权
-   403, Forbidden，禁止访问
-   404, Not Found，未发现指定网址
-   500, Internal Server Error，服务器发生错误

#### statusText

`statusText`属性为只读属性，返回一个字符串，表示服务器发送的状态提示。不同于`status`属性，该属性包含整个状态信息，比如”200 OK“。

#### timeout

`timeout`属性等于一个整数，表示多少毫秒后，如果请求仍然没有得到结果，就会自动终止。如果该属性等于 ==**0**==，就表示**没有时间限制**。

```js
  xhr.ontimeout = function () {
    console.error("The request for " + url + " timed out.");
  }; 
  xhr.open("GET", url, true);
  xhr.timeout = timeout;
  xhr.send(null);
```

####事件监听接口

-   onloadstart 请求发出
-   onprogress 正在发送和加载数据
-   onabort 请求被中止，比如用户调用了`abort()`方法
-   onerror 请求失败
-   onload 请求成功完成
-   ontimeout 用户指定的时限到期，请求还未完成
-   onloadend 请求完成，不管成果或失败

#### withCredentials(跨域)

`withCredentials`属性是一个布尔值，表示跨域请求时，用户信息（比如Cookie和认证的HTTP头信息）是否会包含在请求之中，默认为`false`。即向`example.com`发出跨域请求时，不会发送`example.com`设置在本机上的Cookie（如果有的话）。

*   如果你需要通过跨域AJAX发送Cookie，需要打开`withCredentials`。

```js
xhr.withCredentials = true;
```

*   为了让这个属性生效，服务器必须显式返回`Access-Control-Allow-Credentials`这个头信息。

`.withCredentials`属性打开的话，不仅会发送Cookie，还会设置远程主机指定的Cookie。注意，此时你的脚本还是遵守同源政策，无法 从`document.cookie`或者HTTP回应的头信息之中，读取这些Cookie。



### XMLHttpRequest实例的方法

#### open()

`XMLHttpRequest`对象的`open`方法用于指定发送HTTP请求的参数，它的使用格式如下，一共可以接受五个参数。

```js
void open(
   string method,//表示HTTP动词，比如“GET”、“POST”、“PUT”和“DELETE”。
   string url,//表示请求发送的网址。
   optional boolean async, //true表示异步请求，false表示同步
   optional string user, //表示用于认证的用户名，默认为空字符串。
   optional string password //表示用于认证的密码，默认为空字符串。
);
```

#### setRequestHeader()

`setRequestHeader`方法用于设置HTTP头信息。该方法必须在`open()`之后、`send()`之前调用。如果该方法多次调用，设定同一个字段，则每一次调用的值会**被合并**成一个单一的值发送。

```js
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.setRequestHeader('Content-Length', JSON.stringify(data).length);
xhr.send(JSON.stringify(data));
```

#### send()

`send`方法用于实际发出HTTP请求。不带参数，就表示HTTP请求只包含头信息。

所有XMLHttpRequest的监听事件，都必须在`send()`方法调用之前设定。

如果请求是异步的（默认为异步），该方法在发出请求后会立即返回。如果请求为同步，该方法只有等到收到服务器回应后，才会返回。

```js
var data = 'email='
  + encodeURIComponent(email)
  + '&password='
  + encodeURIComponent(password);
ajax.open('POST', 'http://www.example.com/somepage.php', true);
ajax.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
ajax.send(data);
```

#### abort()

`abort`方法用来终止**已经发出**的HTTP请求。

```js
ajax.open('GET', 'http://www.example.com/page.php', true);
var ajaxAbortTimer = setTimeout(function() {//在一个请求发出5秒之后，终止请求。
  if (ajax) {
    ajax.abort();
    ajax = null;
  }
}, 5000);
```

#### getAllResponseHeaders()

`getAllResponseHeaders`方法返回服务器发来的**所有**HTTP头信息。格式为**字符串**，每个头信息之间使用`CRLF`分隔，如果没有受到服务器回应，该属性返回`null`。

#### getResponseHeader()

`getResponseHeader`方法返回HTTP头信息**指定字段的值**，如果还没有收到服务器回应或者指定字段不存在，则该属性为`null`。

```js
function getHeaderTime () {
    //如果有多个字段同名，则它们的值会被连接为一个字符串，每个字段之间使用“逗号+空格”分隔。
  console.log(this.getResponseHeader("Last-Modified"));
}
```



### XMLHttpRequest实例的事件

#### readyStateChange事件

`readyState`属性的值发生改变，就会触发readyStateChange事件。

#### progress事件

上传文件时，XMLHTTPRequest对象的upload属性有一个progress，会不断返回上传的进度。

假定网页上有一个progress元素。

```html
<progress min="0" max="100" value="0">0% complete</progress>
```

文件上传时，对upload属性指定progress事件回调函数，即可获得上传的进度。

```js
  var progressBar = document.querySelector('progress');
  xhr.upload.onprogress = function(e) {
    if (e.lengthComputable) {
      progressBar.value = (e.loaded / e.total) * 100;
      progressBar.textContent = progressBar.value; // Fallback for unsupported browsers.
    }
  };
```

#### load事件、error事件、abort事件

```js
var xhr = new XMLHttpRequest();

xhr.addEventListener("progress", updateProgress);
xhr.addEventListener("load", transferComplete);
xhr.addEventListener("error", transferFailed);
xhr.addEventListener("abort", transferCanceled);

xhr.open();
```

#### loadend事件

`abort`、`load`和`error`这三个事件，会伴随一个`loadend`事件，表示请求结束，但不知道其是否成功。

```js
req.addEventListener("loadend", loadEnd);

function loadEnd(e) {
  alert("请求结束（不知道是否成功）");
}
```

### 文件上传

HTML网页的`<form>`元素能够以四种格式，向服务器发送数据。

-   使用`POST`方法，将`enctype`属性设为`application/x-www-form-urlencoded`，这是默认方法。
-   使用`POST`方法，将`enctype`属性设为`text/plain`。
-   使用`POST`方法，将`enctype`属性设为`multipart/form-data`。
-   使用`GET`方法，`enctype`属性将被==忽略==。



通常，我们使用file控件实现文件上传

```html
<form id="file-form" action="handler.php" method="POST">
  <input type="file" id="file-select" name="photos[]" multiple/>
  <button type="submit" id="upload-button">上传</button>
</form>
```

上面HTML代码中，file控件的multiple属性，指定可以一次选择多个文件；如果没有这个属性，则一次只能选择一个文件。

file对象的files属性，返回一个FileList对象，包含了用户选中的文件。

```js
var fileSelect = document.getElementById('file-select');
var files = fileSelect.files;
```

然后，新建一个FormData对象的实例，用来模拟发送到服务器的表单数据，把选中的文件添加到这个对象上面。

```js
var formData = new FormData();

for (var i = 0; i < files.length; i++) {
  var file = files[i];

  if (!file.type.match('image.*')) {
    continue;
  }

  formData.append('photos[]', file, file.name);
}
```

上面代码中的FormData对象的append方法，除了可以添加文件，还可以添加二进制对象（Blob）或者字符串。

```js
//append方法的第一个参数是表单的控件名，第二个参数是实际的值，第三个参数是可选的，通常是文件名。
formData.append(name, file, filename);
```

使用Ajax方法向服务器上传文件。

```js
var xhr = new XMLHttpRequest();

xhr.open('POST', 'handler.php', true);

xhr.onload = function () {
  if (xhr.status !== 200) {
    alert('An error occurred!');
  }
};

xhr.send(formData);
```