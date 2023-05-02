<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta content="IE=edge" http-equiv="X-UA-Compatible" />
    <meta content="text/html; charset=utf-8" />
    <meta
      content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"
      name="viewport"
    />

    <link rel="icon" href="/simwrapper/images/SW_favicon.png" />

    <link href="/simwrapper/css/font-awesome-5.7.1.css" rel="stylesheet" />
    <link href="/simwrapper/images/SW_favicon.png" rel="icon" type="image/png" />

    <!-- descriptive things -->
    <title>SimWrapper</title>
    <meta content="SimWrapper" name="twitter:title" />
    <meta content="SimWrapper" name="og:title" />
    <meta content="From the VSP team at TU-Berlin" name="twitter:description" />
    <meta content="From the VSP team at TU-Berlin" name="og:description" />
    <meta content="https://vsp.berlin/simwrapper" name="og:url" />
    <!-- <meta
      content="https://github.com/matsim-vsp/covid-sim/raw/master/src/assets/images/v1-thumb.png"
      name="og:image"
    /> -->
    <meta content="summary_large_image" name="twitter:card" />
    <meta content="@billyinberlin" name="twitter:site" />
    <meta content="@billyinberlin" name="twitter:creator" />
    <!-- <meta
      content="https://github.com/matsim-vsp/covid-sim/raw/master/src/assets/images/v1-thumb.png"
      name="twitter:image"
    /> -->

    <!-- github pages hack: allows single-page-app to handle arbitrary URLs -->
    <script>
      function getQueryVariableForRedirect(variable) {
        var query = window.location.search.substring(1)
        var vars = query.split('&')
        for (var i = 0; i < vars.length; i++) {
          var pair = vars[i].split('=')
          if (pair[0] == variable) {
            return pair[1]
          }
        }
        return false
      }

      ;(function () {
        console.log(55, window.location.search)

        let numRedirect = getQueryVariableForRedirect('redirect404')
        var redirect = sessionStorage[`redirect${numRedirect}`]
        delete sessionStorage[`redirect${numRedirect}`]

        if (redirect && redirect != location.href) {
          history.replaceState(null, null, redirect)
        }
      })()
    </script>

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <!-- <link
      href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,300;0,700;1,300;1,700&family=Space+Grotesk:wght@400;700&display=swap"
      rel="stylesheet"
    /> -->
    <!-- <link
      href="https://fonts.googleapis.com/css2?family=Roboto+Condensed:ital,wght@0,300;0,700;1,300;1,700&family=Space+Grotesk:wght@400;700&display=swap"
      rel="stylesheet"
    /> -->
    <link
      href="https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,400;0,700;1,400;1,700&family=Space+Grotesk:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <!-- <link
      href="https://fonts.googleapis.com/css2?family=Titillium+Web:ital,wght@0,300;0,700;1,300&display=swap"
      rel="stylesheet"
    /> -->
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto+Slab:ital,wght@0,400;0,700;1,300&display=swap"
      rel="stylesheet"
    />
    <script type="module" crossorigin src="/simwrapper/assets/index.5c7ab6e8.js"></script>
    <link rel="modulepreload" href="/simwrapper/assets/vendor.911ae443.js" />
    <link rel="stylesheet" href="/simwrapper/assets/index.851e1cf5.css" />
  </head>
  <body>
    <div id="app"></div>

    <noscript>
      <strong>This site requires JavaScript. Please enable it to continue.</strong>
    </noscript>

    <iframe
      src="https://billyc.github.io/simwrapper/local/sfcta/shapefiles/viz-map-sf-transit.yaml"
      style="height: 100rem; width: 100%; border-radius: 8px"
      title="Iframe Example"
    >
    </iframe>
    <iframe
      src="https://billyc.github.io/simwrapper/local/sfcta/"
      style="height: 100rem; width: 100%; border-radius: 8px"
      title="Iframe Example 2"
    >
    </iframe>

    <iframe
      src="https://billyc.github.io/simwrapper/local/mosaik-2/mosaik-small.xyt.csv"
      style="height: 100rem; width: 100%; border-radius: 8px"
      title="Iframe Example 2"
    >
    </iframe>
  </body>
</html>
