# Numeric.js

The original project [numericjs.com](http://www.numericjs.com) seems to be destoyed. But There is public Github repository [here](https://github.com/sloisel/numeric).

And Cloudfare CDN is still working. The last version maybe 1.2.6 

* https://cdnjs.cloudflare.com/ajax/libs/numeric/1.2.6/numeric.js
* https://cdnjs.cloudflare.com/ajax/libs/numeric/1.2.6/numeric.min.js

There is no official document. So I forked sloisel's repo and deploy document.html via github-page.

* API : [Reference card for the numeric module](https://pineclone.github.io/numeric/documentation.html) 

***

# Applications

## MDS ( Multidimensional Scaling ) with javascript

There is Benfred Erickson's Core code [here](https://github.com/benfred/mds.js) and [examples](https://www.benfrederickson.com/multidimensional-scaling/) of how it works.

```javascript
/// given a matrix of distances between some points, returns the
/// point coordinates that best approximate the distances
mds.classic = function(distances, dimensions) {
    dimensions = dimensions || 2;

    // square distances
    var M = numeric.mul(-.5, numeric.pow(distances, 2));

    // double centre the rows/columns
    function mean(A) { return numeric.div(numeric.add.apply(null, A), A.length); }
    var rowMeans = mean(M),
        colMeans = mean(numeric.transpose(M)),
        totalMean = mean(rowMeans);

    for (var i = 0; i < M.length; ++i) {
        for (var j =0; j < M[0].length; ++j) {
            M[i][j] += totalMean - rowMeans[i] - colMeans[j];
        }
    }

    // take the SVD of the double centred matrix, and return the
    // points from it
    var ret = numeric.svd(M),
        eigenValues = numeric.sqrt(ret.S);
    return ret.U.map(function(row) {
        return numeric.mul(row, eigenValues).splice(0, dimensions);
    });
};
```



***

Numeric Javascript by Sébastien Loisel
======================================

Numeric Javascript is a javascript library for doing numerical
analysis in the browser. Because Numeric Javascript uses only the
javascript programming language, it works in many browsers and does
not require powerful servers.

Numeric Javascript is for building "web 2.0" apps that can perform
complex calculations in the browser and thus avoid the latency of
asking a server to compute something. Indeed, you do not need a
powerful server (or any server at all) since your web app will perform
all its calculations in the client.

For further information, see http://www.numericjs.com/
Discussion forum: http://groups.google.com/group/numericjs

License
-------

Numeric Javascript is copyright by Sébastien Loisel and is distributed
under the MIT license. Details are found in the license.txt file.

Dependencies
------------

There are no dependencies for numeric.js.

Subdirectories
--------------

Here are some of the subdirectories and their contents:

* / holds the html/php files for the workshop/web site, license.txt, README, etc...

* /src holds the source files. The .js files are concatenated together to produce lib/numeric.js

* /lib holds numeric.js and numeric-min.js. These files aren't checked into the git tree because
they are created from the files in the /src subdirectory.

* /resources holds some small images, css files, etc...

* /tools contains build/test scripts. If you're going to patch or otherwise make tweaks to numeric,
you will need to use the /tools/build.sh script. There are also a bunch of scripts to deploy to
the numericjs.com web site, which you probably won't need.

Building and testing
--------------------

If you tweak the code, you can build and test the library by running the script /tools/build.sh. If you plan
to send me patches, please at least run this build script and check that all the tests pass.
