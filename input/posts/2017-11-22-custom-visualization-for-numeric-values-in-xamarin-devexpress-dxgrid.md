---
id: 532
title: Custom Visualization for Numeric Values in Xamarin / DevExpress dxGrid
date: 2017-11-22T17:00:53+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=532
permalink: /2017/11/custom-visualization-for-numeric-values-in-xamarin-devexpress-dxgrid/
categories:
  - xamarin
tags:
  - dotnet
  - app
  - design
  - development
  - layout
  - programming
  - xamarin
---
The dxGrid [grid control for Xamarin](https://components.xamarin.com/view/devexpress-grid), which DevExpress provides free of charge, is a powerful feature for mobile application development. However, many of the functions are a little bit hidden. In this article I will discuss how to customize the display of column values.

In my concrete example my grid contains a numeric column, which can contain numbers with or without decimal places. I want these decimal places to be displayed only if they exist. The magic term that DevExpress provides fortunately is _DisplayFormat_. You can use this parameter to make various adjustments that describe the corresponding value and its representation.

The [documentation of DevExpress](https://documentation.devexpress.com/Xamarin/DevExpress.Mobile.DataGrid.GridColumn.DisplayFormat.property) describes the use of standard or custom numeric format strings, which can be used when using the dxGrid. Both of these types of formatting are documented in the MSDN:

  * [Standard Numeric Format Strings](https://msdn.microsoft.com/en-us/library/dwhawy9k(v=vs.85).aspx)
  * [Custom Numeric Format Strings](https://msdn.microsoft.com/en-us/library/0c899ak8(v=vs.85))

To solve my problem I use the custom formats. The available elements for controlling the display are easy to understand, and in my case I only need the following two elements:

  * 0 to display the number before the decimal point
  * \# for optional display of decimal places

The combination of these elements solves my problem with the following statement:

```csharpString.Format("{0:0.###}", myNumber)```

The prefix 0: ensures that the formatting is only applied to the first argument of the statement and can be omitted in this case, but is useful for concatenated strings.

While this method can be used for all Xamarin controls, the dxGrid also offers the possibility to make adjustments directly in the XAML file:

```xml <dxGrid:GridControl 
    x:Name="grid" 
    <!-- ... -->
    IsReadOnly="true">
    <dxGrid:GridControl.Columns>
        <!-- ... -->
        <dxGrid:NumberColumn 
            FieldName="NumberSample" 
            Caption = "Number" 
            DisplayFormat="0:0.###" />
    </dxGrid:GridControl.Columns>
</dxGrid:GridControl>```