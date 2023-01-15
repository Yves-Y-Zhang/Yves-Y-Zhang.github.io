---
layout:     post
title:      NonBreaking Space and Normal Space
subtitle:   QString, std::wtring and std::string
date:       2023-01-15
author:     Yves
header-img: 
catalog: true
tags:
    - C++ 
    - Qt
    - Issue
---

## Problem
There is a file `A B.dxf` that is failing to import in our application. 

<br /> 

## Debugging
Upon further investigation, I discovered that the space between A and B is a [non-breaking space](https://en.wikipedia.org/wiki/Non-breaking_space) (char 160) in the QString (Unicode), rather than a normal space (char 32). This causes an issue when converting from QString to std::string, as the char 160 is converted to char 63 ('?'), resulting in an import failure. 

![image](../img/20230115/1.png)

<br /> 

## Solution
1. Replace char 160 to char 32 in QString and use [QFile::rename](https://cplusplus.com/reference/string/wstring/) to modify the file name on the cpmputer, but this approach would raise a permission issue as we would need to seek authorization before modifying the user's file. 
2. Modify the data structure of the DXF class by changing the file path type from std::string to [std::wstring](https://cplusplus.com/reference/string/wstring/) using [QString::toStdWString](https://doc.qt.io/qt-6/qstring.html#toStdWString). However, this solution would require significant modifications to the menber variable and function of DXF class and could introduce additional issues.