---
layout: post
title: Getting Started with WPF MVVM
date: 2013-11-24 18:44:53.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- MVVM Light Toolkit
- WPF
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
<blockquote><em>Once a developer becomes comfortable with WPF and MVVM, it can be difficult to differentiate the two. - Josh Smith</em></p></blockquote>
<p>I couldn't agree more with Smith. I actually take it to the extreme and say that if you are working with WPF today without the MVVM design pattern, then you are definitely doing it wrong. Code-behind is the enemy, isolating your components through MVVM increases testability, maintainability and readability of your code. These are all essential in the clean code paradigm, so it would be illogical <em>not</em> to apply MVVM to your WPF code.</p>
<p>In the following guide I explain how you can build a simple Hello World WPF MVVM application. There are <a title="MVVM toolkits" href="http://en.wikipedia.org/wiki/Model_View_ViewModel#Microsoft_.NET_open_source_MVVM_frameworks">many MVVM toolkits</a> out there, I prefer Laurent Bugnion's <a title="MVVM Light Toolkit" href="http://www.galasoft.ch/mvvm/">MVVM Light Toolkit</a> for its light weight capabilities.</p>
<p><strong>Creating a Hello World WPF MVVM Application</strong></p>
<p>In Visual Studio, create a new WPF application project. Go to NuGet, search for and install "Mvvm Light" to the project. You'll see that a "ViewModel" folder was added with two classes inside it; <code>ViewModelLocator</code> and <code>MainViewModel</code>. Delete the latter class as it's just a sample, the former class however is the locator class that WPF uses to data bind views with their respective view models, so we'll need that. Create a "SenderViewModel" class and add it to the ViewModel folder:</p>
<p>[code language="csharp"]<br />
public class SenderViewModel : ViewModelBase<br />
    {<br />
        public RelayCommand OnClickCommand { get; set; }</p>
<p>        public string TextBoxText<br />
        {<br />
            get;<br />
            set<br />
            {<br />
                _textBoxText = value;<br />
                RaisePropertyChanged(&quot;TextBoxText&quot;);<br />
            }<br />
        }</p>
<p>        public SenderViewModel()<br />
        {<br />
            OnClickCommand = new RelayCommand(OnClickCommandAction, null);<br />
        }</p>
<p>        private void OnClickCommandAction()<br />
        {<br />
            var viewModelMessage = new ViewModelMessage()<br />
                {<br />
                    Text = TextBoxText<br />
                };<br />
            Messenger.Default.Send(viewModelMessage);<br />
        }<br />
    }<br />
[/code]<br />
Note the inheritance from <code>ViewModelBase</code>, and the public properties that we will be using to bind to our view. We are also using messaging here (which follows with the toolkit) to send messages from this sender view model. Create a folder called "Messages" and add a "ViewModelMessage" class to it:<br />
[code language="csharp"]<br />
public class ViewModelMessage : MessageBase<br />
    {<br />
        public string Text { get; set; }<br />
    }<br />
[/code]<br />
Note the inheritance from <code>MessageBase</code>. Now go to <code>ViewModelLocator</code> and in the code rename "MainViewModel" to "SenderViewModel", build solution. Create a folder "View" and add a "SenderView" User Control (XAML) to it:<br />
[code language="xml"]<br />
&lt;UserControl x:Class=&quot;WpfApplication1.View.SenderView&quot;<br />
             xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;<br />
             xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;<br />
             xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;<br />
             xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;<br />
             xmlns:i=&quot;http://schemas.microsoft.com/expression/2010/interactivity&quot;<br />
             xmlns:Command=&quot;http://www.galasoft.ch/mvvmlight&quot;<br />
             mc:Ignorable=&quot;d&quot;<br />
             d:DesignHeight=&quot;300&quot; d:DesignWidth=&quot;300&quot; DataContext=&quot;{Binding Source={StaticResource Locator}, Path=SenderViewModel}&quot;&gt;<br />
    &lt;Grid&gt;<br />
        &lt;Label Content=&quot;Sender&quot; Margin=&quot;90,34,0,232&quot;/&gt;<br />
        &lt;TextBox HorizontalAlignment=&quot;Left&quot; Width=&quot;120&quot; Height=&quot;20&quot; Margin=&quot;50,266,0,11&quot; Text=&quot;{Binding TextBoxText}&quot;/&gt;<br />
        &lt;Button Content=&quot;Send&quot; Width=&quot;50&quot; Height=&quot;25&quot; Margin=&quot;183,265,67,10&quot;&gt;<br />
            &lt;i:Interaction.Triggers&gt;<br />
                &lt;i:EventTrigger EventName=&quot;Click&quot;&gt;<br />
                    &lt;Command:EventToCommand Command=&quot;{Binding OnClickCommand}&quot; /&gt;<br />
                &lt;/i:EventTrigger&gt;<br />
            &lt;/i:Interaction.Triggers&gt;<br />
        &lt;/Button&gt;<br />
    &lt;/Grid&gt;<br />
&lt;/UserControl&gt;<br />
[/code]<br />
As you can see, <code>DataContext="{Binding Source={StaticResource Locator}, Path=SenderViewModel}</code> is where the magic happens. The data binding is done here, where the source refers to <code>ViewModelLocator</code> which has the key "Locator" that's automatically referenced inside <code>App</code>. Note how the text box element is bidden to the <code>TextBoxText</code> property that's defined in the view model. Finally, note how a trigger is defined to fire on the click event of the button element, bidden to the <code>OnClickCommand</code> relay command property. Now go to "MainWindow" and add the following XAML code inside the grid tags:<br />
[code language="xml"]<br />
&lt;Grid&gt;<br />
&lt;Grid.RowDefinitions&gt;<br />
            &lt;RowDefinition Height=&quot;Auto&quot; /&gt;<br />
        &lt;/Grid.RowDefinitions&gt;<br />
        &lt;Grid.ColumnDefinitions&gt;<br />
            &lt;ColumnDefinition Width=&quot;Auto&quot;/&gt;<br />
            &lt;ColumnDefinition Width=&quot;Auto&quot;/&gt;<br />
        &lt;/Grid.ColumnDefinitions&gt;<br />
        &lt;view:SenderView Grid.Row=&quot;0&quot; Grid.Column=&quot;1&quot; /&gt;<br />
        &lt;GridSplitter HorizontalAlignment=&quot;Left&quot; Width=&quot;5&quot; Height=&quot;320&quot; Margin=&quot;245,0,0,-21&quot;/&gt;<br />
&lt;/Grid&gt;<br />
[/code]<br />
Build and run solution, the application will start:</p>
<p><a href="http://sirars.files.wordpress.com/2013/11/capture.png"><img class="alignnone size-medium wp-image-124" alt="Capture" src="{{ site.baseurl }}/assets/capture.png?w=300" width="300" height="199" /></a></p>
<p>We have finished the sender part, what remains now is the receiver. Create a "ReceiverViewModel" class and add it to the ViewModel folder:<br />
[code language="csharp"]<br />
 public class ReceiverViewModel : ViewModelBase<br />
    {<br />
        private string _contentText;</p>
<p>        public string ContentText<br />
        {<br />
            get { return _contentText; }<br />
            set<br />
            {<br />
                _contentText = value;<br />
                RaisePropertyChanged(&quot;ContentText&quot;);<br />
            }<br />
        }</p>
<p>        public ReceiverViewModel()<br />
        {<br />
            Messenger.Default.Register&lt;ViewModelMessage&gt;(this, OnReceiveMessageAction);<br />
        }</p>
<p>        private void OnReceiveMessageAction(ViewModelMessage obj)<br />
        {<br />
            ContentText = obj.Text;<br />
        }<br />
    }<br />
[/code]<br />
Note how the receiving message is registered in the constructor. It is important to register for a message type prior to sending the message, otherwise the receiving view model won't receive the sent message, which makes sense. Now go to <code>ViewModelLocator</code> and register the view model you just created:<br />
[code language="csharp"]<br />
public class ViewModelLocator<br />
    {<br />
        public ViewModelLocator()<br />
        {<br />
            ServiceLocator.SetLocatorProvider(() =&gt; SimpleIoc.Default);</p>
<p>            SimpleIoc.Default.Register&lt;SenderViewModel&gt;();<br />
            SimpleIoc.Default.Register&lt;ReceiverViewModel&gt;();<br />
        }</p>
<p>        public SenderViewModel SenderViewModel<br />
        {<br />
            get<br />
            {<br />
                return ServiceLocator.Current.GetInstance&lt;SenderViewModel&gt;();<br />
            }<br />
        }</p>
<p>        public ReceiverViewModel ReceiverViewModel<br />
        {<br />
            get<br />
            {<br />
                return ServiceLocator.Current.GetInstance&lt;ReceiverViewModel&gt;();<br />
            }<br />
        }</p>
<p>        public static void Cleanup()<br />
        {<br />
            // TODO Clear the ViewModels<br />
        }<br />
    }<br />
[/code]<br />
Create a "ReceiverView" User Control and add it to the "View" folder:<br />
[code language="xml"]<br />
&lt;UserControl x:Class=&quot;WpfApplication1.View.ReceiverView&quot;<br />
             xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;<br />
             xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;<br />
             xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;<br />
             xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;<br />
             mc:Ignorable=&quot;d&quot;<br />
             d:DesignHeight=&quot;300&quot; d:DesignWidth=&quot;300&quot; DataContext=&quot;{Binding Source={StaticResource Locator}, Path=ReceiverViewModel}&quot;&gt;<br />
    &lt;Grid&gt;<br />
        &lt;Label Content=&quot;Receiver&quot; Margin=&quot;90,34,0,232&quot;/&gt;<br />
        &lt;Label Content=&quot;{Binding ContentText}&quot; FontSize=&quot;20&quot; Margin=&quot;65,255,40,0&quot;/&gt;<br />
    &lt;/Grid&gt;<br />
&lt;/UserControl&gt;<br />
[/code]<br />
For the final step, go to <code>MainWindow</code> and include the <code>ReceiverView</code>:<br />
[code language="xml"]<br />
&lt;Window x:Class=&quot;WpfApplication1.MainWindow&quot;<br />
        xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;<br />
        xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;<br />
        xmlns:view=&quot;clr-namespace:WpfApplication1.View&quot;<br />
        Title=&quot;MainWindow&quot; Height=&quot;350&quot; Width=&quot;525&quot;&gt;<br />
    &lt;Grid&gt;<br />
        &lt;Grid.RowDefinitions&gt;<br />
            &lt;RowDefinition Height=&quot;Auto&quot; /&gt;<br />
        &lt;/Grid.RowDefinitions&gt;<br />
        &lt;Grid.ColumnDefinitions&gt;<br />
            &lt;ColumnDefinition Width=&quot;Auto&quot;/&gt;<br />
            &lt;ColumnDefinition Width=&quot;Auto&quot;/&gt;<br />
        &lt;/Grid.ColumnDefinitions&gt;<br />
        &lt;view:ReceiverView Grid.Row=&quot;0&quot; Grid.Column=&quot;0&quot; /&gt;<br />
        &lt;view:SenderView Grid.Row=&quot;0&quot; Grid.Column=&quot;1&quot; /&gt;<br />
        &lt;GridSplitter HorizontalAlignment=&quot;Left&quot; Width=&quot;5&quot; Height=&quot;320&quot; Margin=&quot;245,0,0,-21&quot;/&gt;<br />
    &lt;/Grid&gt;<br />
&lt;/Window&gt;<br />
[/code]<br />
Build and run solution, the final product:</p>
<p><a href="http://sirars.files.wordpress.com/2013/11/capture1.png"><img src="{{ site.baseurl }}/assets/capture1.png?w=300" alt="Capture" width="300" height="200" class="alignnone size-medium wp-image-134" /></a></p>
<p>That's pretty much it. In this simple guide, we have demonstrated the use of the MVVM design pattern in WPF, something which any WPF developer should be comfortable to work with. Enjoy!</p>
