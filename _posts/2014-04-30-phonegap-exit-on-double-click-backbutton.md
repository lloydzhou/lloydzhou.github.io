---
layout: post
category : project
tagline: "phonegap exit app when double click backbutton"
tags : [phonegap, javascript]
---
{% include JB/setup %}

# background
> i have made a app build with phonegap.
> i want exit app when double click backbutton.

# solution
> when first click the set exitApp = true
> so second time to click the backbutton will exit the app. 
> but set one Interval to change the status of exitApp to false. 
> so when click backbutton twice in 1 second, will exit the app.

    document.addEventListener('deviceready', function() {
        var exitApp = false, intval = setInterval(function (){exitApp = false;}, 1000);
        document.addEventListener("backbutton", function (e){
            e.preventDefault();
            if (exitApp) {
                clearInterval(intval) 
                (navigator.app && navigator.app.exitApp()) || (device && device.exitApp())
            }
            else {
                exitApp = true
                history.back(1);
            } 
        }, false);
    }, false);

