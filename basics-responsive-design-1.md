# Example for a HTML page with a media query

1. Copy the template below to an empty file and name it e.g. `responsive-example.html`.
2. Open this file in your browser.
3. Open the dev tools and switch to "Responsive mode".
4. Play around with different screen sizes, especially change the width!
5. Watch the background colour of the header.

Interesting articles:
- [MDN - Responsive Design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design)
- [MDN - Beginner's guide to media queries](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Media_queries)


```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Basic HTML Project</title>
        <style>
            /* Basic CSS styles */
            body {
                font-family: Arial, sans-serif;
                background-color: #f0f0f0;
                margin: 0;
                padding: 0;
            }

            header {
                background-color: #333;
                color: #fff;
                text-align: center;
                padding: 20px;
            }

            main {
                padding: 20px;
            }

            /* Media Query for screens with a width of 300px or less */
            @media (max-width: 500px) {
                header {
                    background-color: #ff6600;
                }
            }
        </style>
    </head>
    <body>
        <header>
            <h1>Welcome</h1>
        </header>
        <main>
            <h2>Basics - Responsive web page</h2>
            <p>This is a basic HTML project with some markup and CSS. It includes a media query with a breakpoint at 500px.</p>
            <h3>Recommended to read:</h3>
            <ul>
              <li>
                <a href="https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design">MDN - Responsive Design</a>
              </li>
              <li>
                <a href="https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Media_queries">MDN - Beginner's guide to media queries</a>
              </li>
            </ul>
        </main>
    </body>
  </html>
```