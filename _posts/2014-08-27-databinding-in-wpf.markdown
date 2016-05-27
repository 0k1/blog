---
author: Arun Raj
comments: true
date: 2014-08-27 08:20:03+00:00
layout: post
slug: databinding-in-wpf
title: 'DataBinding In WPF '
wordpress_id: 13
tags:
- Data Binding
- WPF
---
While the MSDN has provided an elaborate article on data binding in WPF (you can find that [here](http://msdn.microsoft.com/en-us/library/ms752347.aspx)), this is my attempt to understand what data binding is and how it is done in WPF. Since this is a vast topic let me just break it down to the main topics in data binding.

1. Introduction to Data Binding  
2. Binding Modes  
3. Binding to Source  
4. The UpdateSourceTrigger Property  
5. Converters in Data Binding  
6. Multi Binding  
7. Validation  
8. Binding to Non-DP  
9. Binding to Collections  
10. Datatemplates and Data triggers  
11. DataTemplate Selectors

Since this is quite a large. Topics after UpdateSourceTrigger will be explained in another post.

**1. Introduction to Data Binding.**

The Main aspect of Data Binding is to interact with data elements and establish a connection between the different properties in WPF. Properties in .NET environment are called CLR properties. Also the UI element to which a CLR property is bound to is called a dependency property. Dependency properties are one of the major aspects of WPF. These are the properties that are registered with the dependency property register. They contain the basic Measure and Arrange overrides that a UI element must do while its value changes in order to reflect on its measurement and arrangement in the UI. So dependency properties can be bound to one another or to a CLR property. The restriction being that the target property in data binding should always be a dependency property. If you think about it, that makes sense right? The target should always be able to handle its changes on its own so as to reflect itself in the UI. Hence all data binding targets should be dependency properties. There are more important stuff about data binding that needs to be addressed. We'll get to them as we go forward with each topic.

**Simple Data Binding.**

***Data Binding between UI elements***

For this part I'm going to use XAML which is a simple markup language that's used to write the UI part of most windows applications. You can try these in Microsoft Visual Studio or any XAML editors like XAMLPAD.

    <Slider Name="mySlider" Value="20" Minimum="0" Maximum="100" Height=" 50"/>TextBlock Name="tb" Text="{Binding ElementName=mySlider, Path=Value,Mode=OneWay}"/>



Here we have two elements, a slider named mySlider and a textblock called tb. The Textproperty of the textbox is bound to the Valueproperty of the slider. As you can see the binding is always set at the target of data binding. Since here we are binding between two UI elements, in the binding statement ElementName and Path are mentioned. Path need not be explicitly mentioned. Ie, we can write the above statement as

    <TextBlock Name="tb" Text="{Binding Value, ElementName=mySlider, Mode=OneWay}"/>


You can always do this binding in the code. One method using Binding class is as follows

    Binding binding = new Binding("Value");
    binding.ElementName = "mySlider";
    tb.SetBinding(TextBox.TextProperty, binding);



**Binding Modes**

If you look at the XAML code above you can see a property named Mode being set during binding. Mode defines the nature of the data binding scenario. Mode can take the following values

1.Default  
2.OneTime  
3.OneWay  
4.OneWayToSource  
5.TwoWay

The operation of default is obvious. There are default binding modes defined for most of the dependency properties in WPF. OneTime binding works only once. Binding clears after onetime execution. OneWay declares that the binding is one way. I.e. it causes changes to the source property to automatically update the target property but the source does not get changed. OneWayToSource does the opposite of OneWay. i.e. causes changes to the target property to automatically update the source property but the target does not get changed. Operation of TwoWay can be understood from the name itself. Source or target get updated while the other one changes.

**Binding To Source**

In most scenarios we'll be using data binding to handle the changes on one element due to another element. But at times we'll need to bind a property to another one on the same element or to its parent element. At such requirements binding to source is useful.

Consider a case where you'd want create a textbox whose width changes as the value inside it changes. We can do that either in code as

    Binding binding = new Binding("Text");  
    binding.ElementName = "tb";  
    tb.SetBinding(TextBox.WidthProperty, binding);

or in XAML as

    <TextBox Text="{Binding RelativeSource={RelativeSource Self}, Path=Width}" Width="100"/>  



RelativeSource values can be modified to access the values of its ancestor or a templatedparent.

I'd like to add an extra point about template binding here. TemplateBinding is one of the methods used in WPF template definition to gain controls to access the values of its parent. When writing different control templates for different controls, sometimes we'd want the value of a template element to vary as the templated element varies.

    <Button Height=" 50" Width=" 100">  
    <Button.Template>  
    <ControlTemplate>  
    <Ellipse Fill="Red" Height="{TemplateBinding Height}" Width="{TemplateBinding Width}"/></ControlTemplate></Button.Template> </Button>  


Here the values of the parent are being accessed by the rectangle using templatebinding. It is to be noted that this is a binding done from parent to child element. The value is being accessed by the child element. If we wanted binding in both ways we'd use sourcebinding in which we'll give RelativeSource={RelativeSource TemplatedParent}. Binding mode can also be specified here to control the flow.  
If you try the first two codes in Binding To Source, you'll see that the XAML version requires you to change the focus in order to reflect the change in the UI. This is because by default the binding UpdateSourceTrigger for a textbox is set to "LostFocus". Lets look at what UpdateSourceTrigger does in the next topic.

**UpdateSourceTrigger Property**

Consider a situation where we have two text boxes and we are creating one mimicking the other. This can be written in XAML as

    <TextBox x:Name="txtbox"/>  
    <TextBox Text="{Binding ElementName=txtbox, Path=Text, Mode=TwoWay}"/>  

Again we encounter the need to change the focus at target for the binding to take place between target and source. To make this work, lets set the UpdateSourceTrigger

    <TextBox x:Name="txtbox"/>  
    <TextBox Text="{Binding ElementName=txtbox, Path=Text, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}"/>  

This would give us two text boxes one reflecting the other.

In the case of two way binding sometimes we need to specify the conditions when the source data is to be updated. For such requirements UpdateSourceTrigger values are set. UpdateSourceTrigger is used in data binding scenarios to specify when the changes in the target property should reflect in the source. It can have the following values

  * Default
  * Explicit
  * LostFocus
  * PropertyChanged

PropertyChanged binds the data as soon as there is any change in the target. LostFocus binds the data from target to source when the target goes out of focus. This will be useful when we wish to grab data from a field when user enters another field. Explicit method tells the CLR that the binding will take place explicitly. Suppose we want to bind a textbox TextProperty to a TextBlock TextProperty on a button click. In such situations UpdateSourceTrigger=Explicit is used. We'll see how explicit calls are made to bind data.

XAML

    <TextBox x:Name="txtbox"/>  
    <TextBox x:Name="txtbox2" Text="{Binding ElementName=txtbox, Path=Text, UpdateSourceTrigger=Explicit, Mode=TwoWay}"/>  
    <Button Click="Button_Click" Content="Update"/>

C#

    private void Button_Click(object sender, RoutedEventArgs e)  
    {  
    BindingExpression be = txtbox2.GetBindingExpression(TextBox.TextProperty);
    be.UpdateSource();  
    }

In the above example the button click updates the source.