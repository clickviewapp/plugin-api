# ClickView Plugin API
Please feel free to create an issue if you have any questions not answered by this documentation.
## Demo
[Codepen demo](https://codepen.io/Shalelol/pen/MWJOGBN)

## Introduction
The ClickView plugin has been developed as a single page web application, designed to be implemented within your platform via two components; an Iframe and a small Events API JavaScript library.
## Iframe
This Iframe creates an interface that allows signed in users to browse their videos and select the video which they wish to embed into a 3rd party platform.
### Iframe example
Here is a simple example of an iframe linking to your web application. (It is strongly recommended that you use a protocol-relative URL) 

```html
<iframe id="clickview-plugin" width="800" height="494" src="https://integrations.clickviewapp.com/plugin?consumerKey=xxxxxx"></iframe>
```

**To use the ClickView Plugin within your platform you will need to obtain a valid Consumer Key and developer credentials from ClickView.**

*NOTE: The minimum size for this Iframe is 570px by 350px, please do not make the iframe any smaller than this, we recommend an optimal size of 800px by 494px*

### Query Parameters
The following options are exposed via the query parameters:

| Parameter | Type | Default | Description |
| --- | ---| --- | --- |
| consumerKey | String | [required] | A unique key that has been provided to you by ClickView and Identifies your platform. If you do not provide a valid key, users will not be able to sign into the Plugin.| 
| singleSelectMode | Boolean | False | Setting this to true will remove the "Add" button from the plugin, allowing you to use a button external to our Plugin's iframe to embed the video into your customer's content |
| sizeSelector | Boolean | True | Setting this to false will remove the size selector. Please use this if you plan to control the size of the user’s embedded video.|
| schoolId | GUID | Null | Please see the [Single Sign On](#single-sign-on) Heading |

# CVEvents API
The CVEvents API is a lightweight JavaScript tool (1.3KB) allows you to communicate with the Iframe containing the ClickView Plugin.
## Download
The CVEvents API is available on our CDN:
http://static.clickview.com.au/cv-events-api/1.0.1/cv-events-api.min.js
Feel free to embed this directly into your platform like so (It is strongly recommended that you use a
protocol-relative URL).
```html
<script src="https://static.clickview.com.au/cv-events-api/1.1.1/cv-events-api.min.js" type="text/javascript"></script>
```
Or alternatively you may download the file and host it directly from your platform.

## Usage
To use the CVEvents API, first create an instance passing in your iframe:
```javascript
var cvEventsApi = new CVEventsApi(document.getElementById('clickviewplugin').contentWindow);
cvEventsApi.on('cv-lms-addvideo', function(event, details) {
    //Your logic to embed the video here
});
```
The details parameter will contain all the information that is needed to embed the video content into
your platform. You will rarely, if ever, need to use the event parameter.
## Function Parameters
### event
A MessageEvent object (you will generally not need to use this object).
See: https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent
### details
Information about the video the user has selected.

| Property | Description |
| --- | --- |
| title | The title of the video the user has chosen. |
| embedHtml | HTML which you may directly insert into your page to display the selected video. |
| thumbnailUrl | A thumbnail image of the selected video. |
| embedLink | A link to our video player, use this in conjunction with the embed property if you wish to generate your own iframe. |
| embed {Object} | <ul><li>autoplay: Boolean indicating if the user expects the video to play automatically on page load.</li><li>width: Width in pixels that user wishes the video to be.</li><li>height: Height in pixel that the user wishes the video to be.</li></ul> | 
| fullLink | A link to this video within our website. Use this if you wish to link users to our video instead of embedding it within your platform. |

# Single Sign On
Some of our customers who use a Sign Sign On system may wish to have their instance of our Plugin slightly customized to take them directly to their SSO Log in page when authenticating with our Plugin.

In order for this to be achieved, the customer will have to obtain a School ID from our support team.

This School ID should then be added to the query parameters of the iframe linking to our hosted web
application:
## Parameter Example
`schoolId=00000000-0000-0000-0000-000000000000`

## Iframe Example
```html
<iframe id="clickview-plugin" width="800" height="494" src="https://integrations.clickviewapp.com/plugin?consumerKey=xxxxxx&schoolId=00000000-0000-0000-0000-000000000000"></iframe>
```
*NOTE: Our plugin will still function normally if the School ID is incorrect, taking the user to the default
sign in page rather than the expected SSO page.*
