---
title: "Customizable vector maps with the Mapbox Qt SDK"
date: 2016-10-04
---

> Link: [https://www.qt.io/blog/2016/10/04/customizable-vector-maps-with-the-mapbox-qt-sdk](https://www.qt.io/blog/2016/10/04/customizable-vector-maps-with-the-mapbox-qt-sdk)

Mapbox is a mapping platform that makes it easy to integrate location into any mobile and online application. We are pleased to showcase the [Mapbox Qt SDK](https://www.mapbox.com/blog/qt-framework-support/) as a target platform for our open source vector maps rendering engine. Our Qt SDK is a key component in [Mapbox Drive](https://www.mapbox.com/blog/drive/), the first lane guidance map designed for car companies to control the in-car experience. The Qt SDK also brings high quality, OpenGL accelerated and [customizable](https://www.mapbox.com/mapbox-studio/) maps to Qt native and QtQuick.

![Mapbox QML Properties](/assets/images/mapbox-qml-example.gif){:width="300"} | ![Mapbox QML Runtime Style](/assets/images/mapbox-qml-runtime-example.gif){:width="300"}
<em class="center" style="text-align:center;">Mapbox QML properties and runtime style examples</em>

The combination of [Qt and Yocto](https://www.mapbox.com/blog/native-gl-yocto/) is perfect for bringing our maps to a whole series of embedded devices, ranging from professional NVIDIA and i.MX6 based boards to the popular Raspberry Pi 3.

As part of our [Mapbox Qt SDK](https://github.com/mapbox/mapbox-gl-native/tree/master/platform/qt), we expose Mapbox GL to Qt in two separate APIs:

* `QMapboxGL` - implements a C++03x-conformant API that has been tested from Qt 4.7 onwards (Travis CI currently builds it using both Qt 4 and Qt 5).
* `QQuickMapboxGL` - implements a Qt Quick (QML) item that can be added to a scene. Because `QQuickFramebufferObject`has been added in Qt version 5.2, we support this API from this version onwards. The QML item interface matches the [Qt Map QML type](http://doc.qt.io/qt-5/qml-qtlocation-map.html) almost entirely, making it easy to exchange from the upstream solution.

_QMapboxGL_ and _QQuickMapboxGL_ solve different problems. The former is backwards-compatible with previous versions of Qt and is easily integrated into pure C++ environments. The latter takes advantage of Qt Quick's modern user interface technology, and is the perfect tool for adding navigation maps on embedded platforms. So far we have been testing our code on Linux and macOS desktops, as well as on Linux based embedded devices.

Mapbox is on a joint effort with the Qt Company to make the Mapbox Qt SDK also available through the official Qt Location module - we are aligning APIs to make sure Mapbox-specific features like runtime styles are available.

_QQuickMapboxGL_ API matches [Qt's Map QML Type](http://doc.qt.io/qt-5/qml-qtlocation-map.html), as you can see from the example below:

```qml
import QtPositioning 5.0
import QtQuick 2.0
import QtQuick.Controls 1.0

import QQuickMapboxGL 1.0

ApplicationWindow {
    width: 640
    height: 480
    visible: true

    QQuickMapboxGL {
        anchors.fill: parent

        parameters: [
            MapParameter {
                property var type: "style"
                property var url: "mapbox://styles/mapbox/streets-v9"
            },
        ]

        center: QtPositioning.coordinate(60.170448, 24.942046) // Helsinki
        zoomLevel: 14
    }
}
```

Mapbox Qt SDK is currently in beta stage. We're continuously adding new features and improving documentation is one of our immediate goals. Your patches and ideas are always welcome!

We also invite you to join us next month at [Qt World Summit 2016](https://www.qtworldsummit.com/) and contribute to Mapbox on [GitHub](https://github.com/mapbox/mapbox-gl-native/).