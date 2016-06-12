---
layout: post
title: Binding Command to Ancestor View Model from View in WPF MVVM
date: 2013-11-06 19:51:06.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- MVVM
- WPF MVVM
- XAML
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
<p>[code language="xml"]<br />
&lt;UserControl x:Class=&quot;View1&quot;<br />
             xmlns:V=&quot;clr-namespace:Views&quot;<br />
             DataContext=&quot;{Binding Source={StaticResource Locator},<br />
             Path=ViewModel1}&quot;&gt;</p>
<p> &lt;V:View2&gt;<br />
       &lt;I:Interaction.Triggers&gt;<br />
          &lt;I:EventTrigger EventName=&quot;MouseDoubleClick&quot;&gt;<br />
              &lt;I:InvokeCommandAction Command=&quot;{Binding DataContext.MouseDownCommand,<br />
                                     RelativeSource={RelativeSource FindAncestor,<br />
                                     AncestorType={x:Type V:View1}}}&quot;/&gt;<br />
          &lt;/I:EventTrigger&gt;<br />
       &lt;/I:Interaction.Triggers&gt;<br />
 &lt;/V:View2&gt;</p>
<p>&lt;/UserControl&gt;<br />
[/code]<br />
Note the binding <code>{Binding DataContext.MouseDownCommand, RelativeSource={RelativeSource FindAncestor, AncestorType={x:Type V:View1}}}</code> which makes the whole thing possible. The command is then defined as usual, in <code>ViewModel1</code>:<br />
[code language="csharp"]<br />
public RelayCommand&lt;MouseEventArgs&gt; MouseDownCommand { get; set; }</p>
<p>public ViewModel1()<br />
{<br />
    MouseDownCommand = new RelayCommand&lt;MouseEventArgs&gt;(OnMouseDown);<br />
}</p>
<p>private void OnMouseDown(MouseEventArgs mouseEventArgs)<br />
{<br />
    //Logic<br />
}<br />
[/code]<br />
And that's it. :)</p>
