# Data Inspector Library Bundle

The Data Inspector Library bundle is a sample JavaScript data visualization solution built with
[Data Inspector Library components](https://developer.here.com/olp/documentation/data-inspector-library/api_reference_typedoc/index.html).
This example demonstrates how to quickly embed data visualization into a pure HTML/JavaScript
single-page application without compilation.

## Getting Started

The instructions below explain how you can easily integrate the Data Inspector Library bundle into
your web page.

### Prerequisites

 To integrate the Data Inspector Library into your web page, you need the following:

- Stable Internet connection
- Simple HTML web page hosted and running on a local or remote web server
- `three.min.js` file, a copy of the [Three.js library](https://threejs.org/), that can be
  downloaded from [GitHub](https://github.com/mrdoob/three.js/blob/r129/build/three.min.js)

To use a local web server, you should install a stable version of [Node.js](https://nodejs.org/).
To start the server, you can use the `serve` module by running these commands from your web page
root folder:

```bash
npm i -g serve
serve
```

or

```bash
npx serve
```

### Implementation

The Data Inspector Library bundle includes:

- `js/data-inspector-bundle.js`: library bundle
- `js/data-inspector-decoder-bundle.js`: decoders bundle
- `js/vendors-bundle.js`: bundle with third-party dependencies
- `js/monaco-editor/editor.worker.bundle.js`: bundle with monaco-editor (required for Plugin Editor)
- `js/monaco-editor/ts.worker.bundle.js`: bundle with monaco-editor (required for Plugin Editor)
- `resources`: folder containing the necessary images and themes
- `LICENSE`
- `HERE_NOTICE`: file with license notices of third-party dependencies
- `README.md`

To include the Data Inspector Library component into your web page:

1. Copy the content of the library bundle ZIP archive into your web page root.
2. Add the `three.min.js` file from THREE.js library into your `js` folder. The Data Inspector
Library uses [Three.js library](https://threejs.org/) version 0.129, you can obtain it from 
   [GitHub](https://github.com/mrdoob/three.js/blob/r129/build/three.min.js).
3. Create an `index.html` at the root folder of your web application.

   As a result, your web page file structure should look like this:

   ```bash
   resources/
      ...
      resources files
      ...
   js/
      monaco-editor/
         editor.worker.bundle.js
         ts.worker.bundle.js
      three.min.js
      vendors-bundle.js
      data-inspector-bundle.js
      data-inspector-decoder-bundle.js
   index.html
   ```

4. Modify `index.html`. At the end of the web page's `body` tag, include the
following:

   ```HTML
    <script src="./js/three.min.js"></script>
    <script src="./js/vendors-bundle.js"></script>
    <script src="./js/data-inspector-bundle.js"></script>
   ```

   > #### Note::
   > All Data Inspector Library components are exposed through the `DI` object.

5. Create a container element for `DataInspector` (the top-level component of the Data Inspector
 Library). It should be placed before the `script` tags:

   ```HTML
   <div id="map-here"></div>
   ```

6. Add fonts to the `head`
   ```HTML
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link rel="prefetch" href="https://fonts.googleapis.com/css2?family=Fira+Mono:wght@400;500;700&display=swap" as="style">
    <link rel="prefetch" href="https://fonts.googleapis.com/css2?family=Fira+Sans:wght@300;400;500;700&display=swap" as="style">
    <link href="https://fonts.googleapis.com/css2?family=Fira+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Fira+Sans:wght@300;400;500;700&display=swap" rel="stylesheet">
   ```
7. Add styles to the `head`
    ```HTML
       <style>
            body {
                margin: 0;
            }
        
            #map-here {
                box-sizing: border-box;
                width: 100vw;
                height: 100vh;
                padding: 5vh 5vw;
            }
        
            #map-here.full-screen {
                padding: 0;
                width: 100%;
                height: 100%;
            }
        </style>
       ```
8. Create an instance of `DataInspector`:

   ```HTML
   <script type="text/javascript">
       var basicConfig = {
           lookupServiceEnvironment: DI.LookupEnvironment.HERE,
           mapView: {
               decoder: {
                  url: "./js/data-inspector-decoder-bundle.js"
               },
               theme:
                   "./resources/normal.reduced.night.json"
           },
           monacoEditorWorkersBasePath: "./js/monaco-editor",
           widgets: {
               authForm: {
                   accessKeyForm: true,
                   localAuthForm: true
               }
           },
           toolbar: {
               languageSelector: true
           }
       };
       var customDataRenderingConfig = {
           enableCoverage: true,
           enableFiltering: true,
           enableInfoPanel: true,
           enableExtendedCoverage: true,
           interactiveDataSource: {
               hrn: "hrn:here:data::olp-here:rib-2",
               layer: "topology-geometry"
           }
       };
       new DI.DataInspector(
           basicConfig,
           document.getElementById("map-here"),
           customDataRenderingConfig
       );
   </script>
   ```

## Sample HTML Code

Below you find a full listing of a sample HTML page with the integrated Data Inspector Library.

```HTML
<head>
    <meta charset="utf-8">
    <title>Data Inspector</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link rel="prefetch" href="https://fonts.googleapis.com/css2?family=Fira+Mono:wght@400;500;700&display=swap" as="style">
    <link rel="prefetch" href="https://fonts.googleapis.com/css2?family=Fira+Sans:wght@300;400;500;700&display=swap" as="style">
    <link href="https://fonts.googleapis.com/css2?family=Fira+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Fira+Sans:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            margin: 0;
        }

        #map-here {
            box-sizing: border-box;
            width: 100vw;
            height: 100vh;
            padding: 5vh 5vw;
        }

        #map-here.full-screen {
            padding: 0;
            width: 100%;
            height: 100%;
        }
    </style>
</head>

<body>
    <div id="map-here"></div>

    <script src="./js/three.min.js"></script>
    <script src="./js/vendors-bundle.js"></script>
    <script src="./js/data-inspector-bundle.js"></script>

    <script type="text/javascript">
        var basicConfig = {
            lookupServiceEnvironment: DI.LookupEnvironment.HERE,
            mapView: {
                decoder: {
                    url: "./js/data-inspector-decoder-bundle.js"
                },
                theme:
                    "./resources/normal.reduced.night.json"
            },
            monacoEditorWorkersBasePath: "./js/monaco-editor",
            widgets: {
                authForm: {
                    accessKeyForm: true,
                    localAuthForm: true
                }
            },
            toolbar: {
                languageSelector: true
            }
        };
        var customDataRenderingConfig = {
            enableCoverage: true,
            enableFiltering: true,
            enableInfoPanel: true,
            enableExtendedCoverage: true,
            interactiveDataSource: {
                hrn: "hrn:here:data::olp-here:rib-2",
                layer: "topology-geometry"
            }
        };
        new DI.DataInspector(
            basicConfig,
            document.getElementById("map-here"),
            customDataRenderingConfig
        );
    </script>
</body>
```

You can serve the content of the folder using the local web server:

   ```bash
   npx serve
   ```

For this sample code to run properly, you should do the following:

1. Verify the names of all included `.js` files
2. Specify `hrn` (your catalog HERE Resource Number) and `layer` (the name of the layer you want to
visualize).
3. To access the HERE platform for China, replace

    ```HTML
       lookupServiceEnvironment: DI.LookupEnvironment.HERE
    ```

   with

      ```HTML
       lookupServiceEnvironment: DI.LookupEnvironment.HERE_CN
      ```

And also  replace hrn property value in customDataRenderingConfig
   
    ```HTML
       hrn: "hrn:here:data::olp-here:rib-2",
    ```

   with

      ```HTML
       hrn: "hrn:here-cn:data::olp-cn-here:here-map-content-china-2",
      ```

4. Connect to the Data Service using the __Access Key ID__ + __Access Key Secret__ pair (optionally
you could also use __Project HRN__).

## License

All license conditions and limitations are described in the `LICENSE` and `HERE_NOTICE` files
distributed together with this `README` and Data Inspector Library JavaScript files.
