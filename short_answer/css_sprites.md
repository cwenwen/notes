# CSS sprites vs data URI

## What are pros and cons of CSS sprites and data URI?

**CSS Sprites**  
 | CSS Sprites | Data URI
 - | - | - 
Description | A collection of images put into a single image | A method for embedding images directly in the HTML or CSS code using base64 encoding
Pros | Reduce the number of server requests and save bandwidth | Save HTTP requests since the images are directly in the codes
Cons | It takes developers loads of time to handle images | The browser can not cache the images, and developers need to re-encode when updating the images