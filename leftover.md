4. Use preprocessing languages like SASS or CoffeeScript
5. Define Asset Fingerprinting


## Preprocessing

Being able to combine files and load them from a set of predefined locations in our application is a great benefit of the Asset Pipeline. That's only the beginning. Because we're loading assets through Rails, we can preprocess the files using popular languages like SCSS for writing better CSS and Coffeescript for cleaner JS. If you make an asset named theme.css.scss, you are telling the asset pipeline to run the file through the SCSS preprocessor before serving theme.css to the browser. The SCSS preprocessor compiles the file into CSS. The only thing we had to do was provide the correct file extension, `.scss`, to the file and the asset pipeline knows to run it through the SCSS preprocessor.

## Fingerprinting

The last benefit we will talk about is Fingerprinting, but first let's talk about the problem it helps us solve. When we serve files to the browser, they are likely to be cached to avoid downloading them again in the future. What's caching you might ask?

Caching something means keeping a copy of a time-consuming operation locally so that you don't have to redo the expensive operation again if the inputs and outputs are going to be exactly the same. Caches are usually key value stores, where the value is the answer to the expensive operation and the key is something that's unique to that item. If you request a page from the server and then request the same page from the server again, the quickest way to get that request fulfilled is to actually keep a copy of what you got last time locally. Browsers cache lots of the responses they get to requests they've made by using the headers that get sent with the response. The headers tell the browser how long the page remains 'fresh' before it 'expires.' Once the page has expired, the browser will make a new request for the page to refresh its cache. We say that the fastest request is the request that's not made. It's also often said that cache invalidation is one of the two hard problems in computer science, so think carefully when you start caching things! Caching saves bandwidth for us and provides a speed boost for the user. This is great until you change the file and you want all of your users to get the new one instead of the old version they have stored in their browser cache. But how do we let the browser know we've modified the file? If the new version has the same name as the old version, the browser will continue using the old file from its cache. We need a way to change the filename when the contents change so that browsers won't keep serving the old file.

### From the [Rails Guides Primer](http://guides.rubyonrails.org/asset_pipeline.html#what-is-fingerprinting-and-why-should-i-care-questionmark)
"Fingerprinting is a technique that makes the name of a filename dependent on the contents of the file. When the file contents change, the filename is also changed. For content that is static or infrequently changed, this provides an easy way to tell whether two versions of a file are identical, even across different servers or deployment dates.

When a filename is unique and based on its content, HTTP headers can be set to encourage caches everywhere (whether at CDNs, at ISPs, in networking equipment, or in web browsers) to keep their own copy of the content. When the content is updated, the fingerprint will change. This will cause the remote clients to request a new copy of the content. This is known as cache busting.

The technique sprockets uses for fingerprinting is to append a hash of the content to the end of the file name. For example, take a CSS file named `global.css`. Sprockets will add the hash `908e25f4bf641868d8683022a5b62f54` to the end of the file name like so:
```
global-908e25f4bf641868d8683022a5b62f54.css
```

If you happen to be using an older version of Rails (Rails 2.x), the strategy used to be to append a date-based query string to every asset linked with a built-in helper. This looked like so:
```
global.css?1309495796
```

The query string strategy has several disadvantages:

- Not all caches will reliably cache content where the filename only differs by query parameters.
    + Steve Souders recommends "avoiding a querystring for cacheable resources." 5-20% of your requests will not be cached. Query strings in particular do not work at all with some CDNs for cache invalidation.
- The file name can change between nodes in multi-server environments.
    + The default query string in Rails 2.x is based on the modification time of the files. When assets are deployed to a cluster, there is no guarantee that the timestamps will be the same, resulting in different values being used depending on which server handles the request.
- Too much cache invalidation.
    + When static assets are deployed with each new release of code, the mtime (time of last modification) of all these files changes, forcing all remote clients to fetch them again, even if the content of those assets has not changed.

Fingerprinting fixes all these problems by ensuring that filenames are consistent based on their content.

Fingerprinting is enabled by default for production and disabled for all other environments. You can enable or disable it in your configuration through the `config.assets.digest` option."

Finally, definitely check out the [keynote where DHH introduces the asset pipeline](https://www.youtube.com/watch?v=cGdCI2HhfAU).

## Resources

* [Rails Guides Primer](https://github.com/learn-co-curriculum/what-is-the-asset-pipeline/edit/master/README.md) 
