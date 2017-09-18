[Full source code on github][1]

In my [Volley Request Manager][2] article I've described model that covers a lot of stuff. But in practice you need only half of it or even less. So I've decided to create this version, that is :

- Reusable
- So simple as possible
- With possibility to include HTTP Client and Image Loader or only one of them

## Description

Library consists of two simple packages: **http** and **utils**. They are independent.

**http**  contains only two independent classes :

- `ImageManager` - for image loading
- `RequestManager` - for requests processing

**utils** contains classes that will help you to create your own `Requests`, `Queues` or `ImageLoaders`. You can use them if you wish

## Usage

Include [library][2] or just copy component that you need from **http** package in your project.

```java
// init component for request processing
RequestManager.initializeWith(getApplicationContext());

// create request
Request request = new JsonObjectRequest(
        Request.Method.GET,
        "request url here",
        null,
        mListener,
        mErrorListener);

// process request with default queue      
RequestManager.queue().add(request);
```

```java
// init component for image loading
ImageManager.initializeWith(getApplicationContext());

// load image with default ImageLoader
ImageManager.loader().get(
        "http://farm6.staticflickr.com/5475/10375875123_75ce3080c6_b.jpg",
        mImageListener);

// load image with NetworkImageView
NetworkImageView view = new NetworkImageView(context);

view.setImageUrl(
        "http://farm6.staticflickr.com/5475/10375875123_75ce3080c6_b.jpg",
        ImageManager.loader()); // to use default ImageLoader
```

## Tips

- Create `HttpUtils` class to hold all your url, headers creation methods in one place

```java
public class HttpUtils {

    public static String createTestUrl() {
        Uri.Builder uri = new Uri.Builder();
        uri.scheme("http");
        uri.authority("httpbin.org");
        uri.path("get");
        uri.appendQueryParameter("name", "Jon Doe");
        uri.appendQueryParameter("age", "21");

        return uri.build().toString();
    }
}
```

- Create `HttpFactory` class to hold all your reusable requests

```java
public class HttpFactory {

    public static void createTestRequest(Response.Listener<JSONObject> listener,
            Response.ErrorListener errorListener) {
        Request request = new JsonObjectRequest(
                Request.Method.GET,
                HttpUtils.createTestUrl(),
                null,
                listener,
                errorListener);
        RequestManager.queue().add(request);
    }

    public static void loadImageWithDefaultStub(String url, NetworkImageView view) {
        view.setDefaultImageResId(R.drawable.ic_launcher);
        view.setImageUrl(
                url,
                ImageManager.loader());
    }
}
```

  [1]: https://github.com/yakivmospan/volley-request-manager-lite
  [2]: https://github.com/yakivmospan/yakivmospan/blob/master/articles/android/http/Volley%20Request%20Manager.md
