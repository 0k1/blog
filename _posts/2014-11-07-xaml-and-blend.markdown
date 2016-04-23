---
author: Arun Raj
comments: true
date: 2014-11-07 06:55:45+00:00
layout: post
slug: xaml-and-blend
title: XAML -Extensible Application Markup Language
wordpress_id: 91
---

XAML stands for Extensible Application Markup Language. It is the stuff under all windows UI we see today. Win8 Store apps, Win phone apps and now with the introduction of windows 10 we'll see XAML in xbox application development too. The [MSDN article](http://msdn.microsoft.com/en-in/library/cc295302.aspx) and [wiki ](http://en.wikipedia.org/wiki/Extensible_Application_Markup_Language)are nice reads if you want to know more about XAML. Majority of the applications in windows are now developed with WPF.


WPF is based on managed code but uses a markup language, Extensible Application Markup Language (XAML), to make building applications much easier for designers. XAML-based applications currently support C# and Microsoft Visual Basic .NET.


XAML enables complex designs to be implemented for an application's UI. It introduces concepts like Styles, Templates, Triggers, Visual states etc. These concepts allow the developers to truly take control of each elements and define their behavior in the applications UI. Hence complex behavior of elements can be defined using XAML. Lets see how a control in WPF is styled templated and have its visual states set with the help of Blend for Visual Studio.

**Styling a control. **

Lets have a look at the following textbox


`<TextBox Background="Transparent" ` `FontSize="{StaticResource MediumFontSize}" ` `Height="40" BorderThickness="0" Margin="0,-1,0,0" ` `Foreground="{StaticResource TaskItemPrimaryTextBrush}"  ` `Text="{Binding Path=SearchTexts, Mode=TwoWay}" ` `HorizontalAlignment="Stretch" />`


This textbox control has a lot of properties set on it.  XAML provides lot of properties that can be set on controls in order to make it look like the design we  require. Now these properties can be individually set on the controls or we can define a style and apply the style to the text box control. Styles can be defined in any resource collection. Ie, we can define a style say in control level or in page level or application level. To define it in a control level we define the style in the resource collection of that control. If we want to alter properties text boxes within a grid only we'd define the style that targets textboxes within the Grid.Resources collection. If we want to alter the style of all textboxes within the application. We'll write the style within the Application.Resources collection. If property values are set using multiple styles then WPF follows a priority rule to decide decide which value gets assigned in the end.

**Step 1**: Determine Base Value

In the order of diminishing priority:

(a) Local Value
(b) Style triggers
(c) Template Triggers
(d) Style Setters
(e) Theme Style Triggers
(f) Theme Style Setters
(g) Inheritance
(h) Default Value

**Step 2**: Evaluate – If Step 1 returns expression (DataBinding or DynamicResource), evaluate the expression and fix the value
**Step 3**: Apply Animation. If animation properties are set, they take more preference.
**Step 4**: Coerce - If CoerceValueCallback defined that value gets assigned.
**Step 5**: Validate - If ValidateValueCallback defined then value after validation gets assigned. What we should note at such a situation is that fundamentally we are setting values for dependency properties. The rules that apply to a dependency property also gets applied here. And yeah, the above lines are from [Jim Mangaly](http://jimmangaly.blogspot.in/). Most of what I write about WPF is from him so copying didn't seem like a bad idea at the time. Thanks Jim :)

Another important fact is the behavior of a resource collection. A resource collection consists of pair of keys and values. Styles we create are also pair of keys and values.


`<Style x:Key="TasksGridViewItemStyle" TargetType="GridViewItem"> <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" /> <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}">
<Setter Property="Margin" Value="0,0,10,10" /> <Setter Property="VerticalContentAlignment" Value="Top" /> </Style>`


The above style targets grid view items. If we were to create such a style without a key . Ie


`<Style TargetType="GridViewItem"> ..... </Style>`


Then the style gets applied to all the GridViewItems within the scope of that resource file. This comes handy when we want to style all elements in a particular context.
