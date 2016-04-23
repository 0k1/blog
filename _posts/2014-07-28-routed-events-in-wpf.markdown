---
author: Arun Raj
comments: true
date: 2014-07-28 12:39:19+00:00
layout: post
slug: routed-events-in-wpf
title: Routed Events in WPF
wordpress_id: 10
---

Routed Events are one of the key concepts of WPF. It enables events to be handled at parent levels. Routed events can be handled at Parent levels of its Visual Tree. Consider a case when you have a custom grid filled with two rectangles and you'd want to initiate a handler when  you press the on any element of the grid. If routing wasn't there you'd have to write click event handles to both the rectangles and direct them to a single handler. If the grid consisted of complex graphic elements the task would be tedious. But since WPF supports Routed Events we can write handlers on the grid control itself and it'd be able to catch any events happening inside. That is the event can be handled at parent level of the element raising the event. This is the key concept of routed events




Let's see the different Events in WPF in detail. There are 3 types of events in WPF.




1.Direct Events




2.Tunneled Events




3.Bubbled Events




**Direct Events**




Direct Events are similar to the normal CLR events in C#. They are defined for an object and it is capable of raising the event by itself. Event handlers are written on the element raising the event itself.




**Tunneled Events.**




Tunneled events are one of the routed events present in WPF. Consider a case when a grid containing a button has a button click event at the button level. The first event handler that is being fired is not the event handler at the button level, but the PreviewMouseLeftKeyDown event at the grid level. Ie, The event can be handled even before reaching the event triggering element. This concept can be used in many ways. Consider a situation where you want only some of the characters entered from the keyboard to be reflected in your textbox. You can set event handled parameter to true in a  PreviewKeyDown Event and thereby cancelling the event from taking place at all. Now Lets see how a Preview event is written in XAML and C#



`
<Grid Name="grid" UIElement.PreviewMouseLeftButtonDown="grid_TunnelHandler">
<Rectangle PreviewMouseLeftButtonDown="Rect_TunnelHandler" Height="100" Width="100"/>
</Grid>
`



Here first the grid_TunnelHandler is fired first and  then Rect_TunnelHandler. The C# Code for the event handlers will be as



`
private void Rect_TunnelHandler(object sender, MouseButtonEventArgs e)
{
}
`

At any level we can set

```
e.handled=true;
```


to handle the event from taking place.

Now consider a situation where we'd want to make the routing to continue even if it has a e.handled value set to true. For this we have to go back to C# and write the handler ourselves.

The method to add a handler is as
`
grid.AddHandler(UIElement.PreviewMouseLeftButtonDown, new RoutedEventHandler(grid_TunnelHandler), (bool) ContinueIfHandled);
`

If the final parameter is set to true then the event will continue routing even if e.Handled is set  to true.

**Registering A Routed Event**
`
public static readonly RoutedEvent MyRoutedEvent=EventManager.RegisterRoutedEvent("AnyNameToReferInXaml", RoutingStrategy.Bubble/Tunneled, typeof(EventHandlerFunction),typeof(ParentClass));
`

A custom routed event can be registered in the WPF Routed events list as above.
