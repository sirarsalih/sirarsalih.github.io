---
layout: post
title: Binding Command to Ancestor View Model from View in WPF MVVM
date: 2013-11-06 19:51:06.000000000 +01:00
type: post
published: true
status: publish
categories:
tags: [dotnet]
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p>Half a year ago while I was working on a WPF MVVM application, I came across a situation where I had placed a view on top of another view and wanted to trigger a command defined in the underlying view's view model. Now you may be wondering why I wanted to bind to a command defined in a different (than the current view's) view model, the answer to that is pretty much architectural. We wanted to use that specific view model for that kind of logic. Using <a title="MVVM Light Toolkit" href="http://www.galasoft.ch/mvvm/">MVVM Light Toolkit</a> by Laurent Bugnion, I thought I'd share with you how I did this. Consider <code>View2</code> is placed on top of <code>View1</code>, both having <code>ViewModel2</code> and <code>ViewModel1</code> as their respective models. The XAML code to bind and trigger the command then looks like this:</p>
```xml
<UserControl x:Class="View1"
xmlns:V="clr-namespace:Views"
DataContext="{Binding Source={StaticResource Locator},
Path=ViewModel1}">

<V:View2>
<I:Interaction.Triggers>
<I:EventTrigger EventName="MouseDoubleClick">
<I:InvokeCommandAction Command="{Binding DataContext.MouseDownCommand,
RelativeSource={RelativeSource FindAncestor,
AncestorType={x:Type V:View1}}}"/>
</I:EventTrigger>
</I:Interaction.Triggers>
</V:View2>

</UserControl>
```
Note the binding ```{Binding DataContext.MouseDownCommand, RelativeSource={RelativeSource FindAncestor, AncestorType={x:Type V:View1}}}``` which makes the whole thing possible. The command is then defined as usual, in ```ViewModel1```:

```csharp
public RelayCommand<MouseEventArgs> MouseDownCommand { get; set; }

public ViewModel1()
{
  MouseDownCommand = new RelayCommand<MouseEventArgs>(OnMouseDown);
}

private void OnMouseDown(MouseEventArgs mouseEventArgs)
{
  //Logic
}
```

And that's it. :)
