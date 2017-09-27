# Web browsers
The main desktop application in use today is the web browser.
In this document we explore some of its inner workings.
The main reference for the below is [How Browsers Work: Behind the scenes of modern web browsers by Tali Garsiel and Paul Irish](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/).

Many companies develop web browsers.
Google have Chrome, Mozilla have Firefox, Apple have Safari, Opera Software have Opera, and Microsoft have Edge.
All of these browsers have one main job, to fetch HTML and render it for the end user.
They also fetch:

- **JavaScript** which is mainly used to manipulate the HTML document,
- **CSS** which is mainly used for styling the HTML document, and
- **images** which are displayed as part of the HTML document.

#### The location bar

Consider a simple webpage like the following.

![A simple webpage](../images/simple-webpage.png)

The webpage is hosted at the URL [https://data-representation.github.io/simple-website/index.html](https://data-representation.github.io/simple-website/index.html).
The user has visited the page by typing this URL into the location bar at the top of the window, and pressing enter.
The file requested is a HTML file, but what is displayed in the browser windows contains images, has had some CSS applied to it from a separate CSS file, and the browser has loaded three JavaScript files.
How did all of that happen?

1. The browser sent a **request** to the server located at `data-representation.github.io` using the `HTTP` protocol on port `80` using the `GET` method, requesting the *resource* named `/simple-website/index.html`.
2. The server sent a **response** to the browser, which indicated that it had successfully located that resource with status code `200`, and the response contained the resource itself which was in the form of HTML.
3. The browser parsed the HTML, and figured out that it needed to also download an image file called `img/logo.png`, a CSS file called `css/bootstrap.min.css`, and three JavaScript files called `js/jquery-3.2.1.slim.min.js`, `js/popper.min.js`, and `js/bootstrap.min.js`.

![Simple webpage source](../images/simple-webpage-source.png)

4. The HTML did not tell the browser on which server to find these files, so it assumed it was the same one that `index.html` came from. For instance, it figured out that `img/logo.png` was really located at `http://data-representation.github.io/img/logo.png`.
5. The browser sent out five separate HTTP `GET` requests for each of those files, and parsed them.

![Chrome loading page](../images/simple-webpage-network-chrome.png)

6. As quickly as it could, it rendered the result of all six files in the browser window. Whenever possible, it did things in parallel, and without waiting for all the resources to download fully.

#### Clicking links, back buttons, and bookmarks
The procedure when a user clicks a link is the same, only for the way the initial `GET` request is initiated by the user.
Each link, assuming its not an internal page anchor beginning with `#`, is really just a URL.
When a user clicks a link, they get the same result as if they had typed the link's URL into the location bar and pressed enter.

The same applies for your browser's back button, which is just a stack of URLs previously visited.
When the back button is clicked, it's as if the user just typed the previous page's URL in the location bar.
The only caveat here is that your browser will often remember extra stuff about the previous page, such as the point to which you had scrolled in it.
If you type the URL in the location bar it will forget all of that stuff.

Likewise, pressing the refresh button is just a convenient way of re-typing the URL in the location bar. 
Likewise, bookmarks are just links that your browser has stored that you can click on instead of typing them into the location bar.
You might notice slight differences in your own browser, depending on how you initiate it, but the result is a `GET` request being initiated by your browser for a URL.
Anything on top, like caching, of that is just a convenience your browser provides.

#### Forms



#### AJAX