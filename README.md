# API Documentation

The gifs API makes it easy to integrate our functionality into your apps.

## Media Routes

The media routes allows anyone to add short media from various sources including external URLs and uploading from disk. There is a `150mb` size and `30s` time limit for the source file and we accept the following image content types:

* `image/gif`
* `video/mp4`
* `video/webm`

When using the `/import` endpoint you can link to `giphy`, `imgur`, `gfycat`, `streamable`, `instagram`, `twitter`, `facebook`, `vine` html URLs instead of the actual media and we'll automagically find the best quality to import.

### Import `POST https://api.gifs.com/media/import`

If the media is already online then you can simply send us the URL to add any media that we support using a POST request with the following schema as `content-tye: application/json`:


| Parameter       | Type         | Required?  | Description                          |
| -------------   |--------------|------------|--------------------------------------|
| source          | string       | required   | The source URL of the media to add   |
| title           | string       | optional   | The title of the media               |
| tags            | []string     | optional   | The tags relating the the media      |
| nsfw            | boolean      | optional   | If the media is not safe for work    |
| attribution     | object       | optional   | Any attribution you want displayed   |

Attribution object schema is:

| Parameter       | Type         | Required?  | Description                                 |
| -------------   |--------------|------------|---------------------------------------------|
| site            | string       | required   | twitter, reddit, instagram, or a custom URL |
| user            | string       | optional   | The username for social media sites         |
| url             | string       | optional   | The url if site != social media             |

#### Example Request

```HTTP
POST /media/import HTTP/1.1
Host: api.gifs.com
Token: beta-access-token-monty
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
{
 "source": "https://45.media.tumblr.com/72fc3bfa5598d0b2bafcc118c651356e/tumblr_mnp5ohGCqK1rmszj9o1_400.gif",
 "title": "Cat does a front flip",
 "tags": ["cat", "flipping", "cat flip", "amazing cats"],
 "attribution": {
   "site": "twitter",
   "user": "montanaflynn"
 }
}
```

#### Example Response

```HTTP
HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8
{
 "success":{
    "page": "https://gifs.com/gif/wpk4Lw",
    "files": {
        "gif": "https://j.gifs.com/wpk4Lw.gif",
        "jpg": "https://j.gifs.com/wpk4Lw.jpg",
        "mp4": "https://j.gifs.com/wpk4Lw.mp4",
        "webm": "https://j.gifs.com/wpk4Lw.webm"
    },
    "embed": "<iframe src='https://gifs.com/embed/wpk4Lw' frameborder='0' scrolling='no' width='309' height='291' style='-webkit-backface-visibility: hidden;-webkit-transform: scale(1);' ></iframe>",
    "meta": {
        "duration": "2.61s",
        "frames": "26",
        "height": "291",
        "width": "309"
    }
 }
}
```

### Upload Endpoint `POST https://api.gifs.com/media/upload`

You can upload your media by sending a `multipart/form-data` POST request with the following form fields:

| Parameter         | Type      | Required?  | Description                                 |
| ------------------|-----------|------------|---------------------------------------------|
| file              | file      | required   | The file being uploaded                     |
| title             | string    | optional   | The title of the media                      |
| tags              | []string  | optional   | The tags relating the the media             |
| nsfw              | boolean   | optional   | If the media is not safe for work           |
| attribution_site  | string    | optional   | twitter, reddit, instagram, or a custom URL |
| attribution_user  | string    | optional   | The username for social media sites         |
| attribution_url   | string    | optional   | The url if site != social media             |

#### Example Request

```shell
curl \
  -F "file=@/media/gifs/awesome.gif" \
  -F "title=The most awesome gif eva" \
  -F "tags=awesome, cool, fun, gnarly" \
  https://api.gifs.com/media/upload
```

#### Example Response

```HTTP
HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8
{
 "success":{
    "page": "https://gifs.com/gif/2k9BMv",
    "files": {
        "gif": "https://j.gifs.com/2k9BMv.gif",
        "jpg": "https://j.gifs.com/2k9BMv.jpg",
        "mp4": "https://j.gifs.com/2k9BMv.mp4",
        "webm": "https://j.gifs.com/2k9BMv.webm"
    },
    "embed": "<iframe src='https://gifs.com/embed/2k9BMv' frameborder='0' scrolling='no' width='640' height='360' style='-webkit-backface-visibility: hidden;-webkit-transform: scale(1);' ></iframe>",
    "meta": {
        "duration": "4.104s",
        "frames": "123",
        "height": "360",
        "width": "640"
    }
 }
}
```
