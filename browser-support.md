## Browser Usage Guidelines

### History
The Ozone Widget Framework has supplied a mechanism for cross-document messaging since 2008, well before the [W3C Web Messaging implementation and specification](http://www.w3.org/TR/webmessaging/). As a result, the issue of browser support has been critical to the lifecycle of the application.

Additionally, OZONE made use of several older methods for cross-document messaging, such as the [window.name method](https://developer.mozilla.org/en-US/docs/Web/API/Window.name). Since the goal of the program is to support W3C and other web standards as much as possible, this team will begin to modernize and deprecate some existing methods due to increased browser support.

From a support perspective, this begins to deprecate support for older browsers, as documented below.

### Support

Support for client-side communications and rendering technologies will now [be mapped to W3C specifications](http://caniuse.com/) versus browsers. The following features are targeted as a minimum entry for usability:

1. [LocalStorage](http://caniuse.com/namevalue-storage) - Used as a storage area for cross-document messaging and message management.
2. [Web Workers](http://caniuse.com/#feat=webworkers) - Used to remove process-intensive functions from the primary rendering thread (optional, will make use where available).
3. [Custom / Extended HTML tags](http://caniuse.com/#feat=html5semantic) - Used to define a better DOM structure for the presentation capability, and allows for better custom integration across the user interface.
4. [PostMessage](http://caniuse.com/#feat=x-doc-messaging) - The primary method for cross-document communication.

### Browser Support Notes

Support for the following browsers will be limited due to the features listed above:

1. LocalStorage - IE8 and above support LocalStorage, but due to a lack of support for other features, this becomes an issue.
2. Web Workers - Since these are used as failover for performance (meaning their usage is not required), and polyfills exist for this functionality, browser support is not limited by this feature.
3. Custom / Extended HTML tags - AngularJS 1.3 and beyond removes support for IE8. Since the presentation is being migrated to AngularJS, support for IE8 will be hindered.
4. PostMessage - While supported in IE8 and above (such as LocalStorage above), the lack of other features in IE8 creates a gap for support.
