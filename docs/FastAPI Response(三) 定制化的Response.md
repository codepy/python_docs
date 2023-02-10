默认情况下，FastAPI会基于*`JSONResponse`*来返回Response。

如果我们直接返回*`Response`*，数据格式不会被自动转换，并且交互式文档也不会自动生成。

下面是一些常用的Response类型。

#### Response

Response主类，所有其他的Response都继承自这个类。

它接收以下参数信息：

- `content` - `str` 或者 `bytes`.
- `status_code` - HTTP 状态码.
- `headers` - 字符串字典.
- `media_type` - media type. 例如`"text/html"`.

FastAPI会自动包含Content-Length，以及Content-Type，charset等头信息。

```
from fastapi import FastAPI, Response

app = FastAPI()


@app.get("/legacy/")
def get_legacy_data():
    data = """<?xml version="1.0"?>
    <shampoo>
    <Header>
        Apply shampoo here.
    </Header>
    <Body>
        You'll have to use soap here.
    </Body>
    </shampoo>
    """
    return Response(content=data, media_type="application/xml")
```

####  

我们可以在路径操作装饰器中声明要返回的具体*`Response`*类型，返回的内容会被放入到指定的*`Response`*`中`。

并且如果我们指定的*`Response`*`支持JSON media类型，例如*`JSONResponse`*或者*`UJSONResponse`*，那么返回的数据就会被自动转换成Pydantic模型(通过*`response_model`*指定)。`

#### `HTMLResponse`

接收文本或者字节内容，返回HTML reponse。

```
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()


@app.get("/items/", response_class=HTMLResponse)
async def read_items():
    return """
    <html>
        <head>
            <title>Some HTML in here</title>
        </head>
        <body>
            <h1>Look ma! HTML!</h1>
        </body>
    </html>
    """
```

#### `PlainTextResponse`

接收文本或者字节内容，返回纯文本 reponse。

```
from fastapi import FastAPI
from fastapi.responses import PlainTextResponse

app = FastAPI()


@app.get("/", response_class=PlainTextResponse)
async def main():
    return "Hello World"
```

#### `JSONResponse`

返回`application/json`格式的response。FastAPI默认返回的response。

#### `RedirectResponse`

返回HTTP重定向。默认状态码为307(临时重定向)。

```
from fastapi import FastAPI
from fastapi.responses import RedirectResponse

app = FastAPI()


@app.get("/typer")
async def read_typer():
    return RedirectResponse("https://typer.tiangolo.com")
```

#### `StreamingResponse`

接收一个异步的发生器或者普通的发生器/枚举器，对返回结果流式输出。

```
from fastapi import FastAPI
from fastapi.responses import StreamingResponse

app = FastAPI()


async def fake_video_streamer():
    for i in range(10):
        yield b"some fake video bytes"


@app.get("/")
async def main():
    return StreamingResponse(fake_video_streamer())
```

如果我们有一个file-like对象(比如用open()打开)，我们就可以用*`StreamingResponse`*来返回该对象。

```
from fastapi import FastAPI
from fastapi.responses import StreamingResponse

some_file_path = "large-video-file.mp4"
app = FastAPI()


@app.get("/")
def main():
    file_like = open(some_file_path, mode="rb"return StreamingResponse(file_like, media_type="video/mp4")
```

#### `FileResponse`

异步流式输出一个文件。

不同于其他response的初始化参数信息：

- `path` - 文件路径
- `headers` - 定制头信息，字典格式
- `media_type` - media type，如果没有设置则会根据文件名或文件路径来推断media type
- `filename` - 文件名。如果设置，会被包含到response的`Content-Disposition中`

文件response会包含合适的`Content-Length`, `Last-Modified` 以及 `ETag` 头信息内容。

```
from fastapi import FastAPI
from fastapi.responses import FileResponse

some_file_path = "large-video-file.mp4"
app = FastAPI()


@app.get("/")
async def main():
    return FileResponse(some_file_path)
```

 

#### `缺省response类`

我们可以指定缺省response类，如下我们指定了*`ORJSONResponse`*为缺省使用的response类。

```
from fastapi import FastAPI
from fastapi.responses import ORJSONResponse

app = FastAPI(default_response_class=ORJSONResponse)


@app.get("/items/")
async def read_items():
    return [{"item_id": "Foo"}]
```