---
layout: post
title: Creating a Cross-Platform App for Windows Phone 8 and Windows Store (PCL, MvvmCross)
date: 2013-12-27 13:46:14.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- ".NET"
- MVVM
- MvvmCross
- PCL
- Portable Class Library
- Visual Studio
- Windows Phone 8
- Windows Store
- Xamarin
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
<p>If you haven't checked it out yet, <a href="http://msdn.microsoft.com/library/gg597391.aspx" title="Portable Class Library">Portable Class Libraries (PCLs)</a> make it possible to write cross-platform .NET applications targeting multiple platforms and using the same code logic base. Combining this with <a href="http://xamarin.com/?gclid=CI65ktum0LsCFYFe3godU08A-g" title="Xamarin">Xamarin</a>, you can target other platforms such as iOS, Android and Mac. That's right, it means you can write C# code for those platforms. Pretty amazing, right? PCL comes with Visual Studio 2012. In the following tutorial, I explain how you can create a cross-platform app for Windows Phone 8 and Windows Store, using PCL and the <a href="https://github.com/MvvmCross/MvvmCross" title="MvvmCross">MvvmCross</a> framework to separate concerns.</p>
<p><strong>Setting up the Environment</strong></p>
<p>PCL comes out of the box with Visual Studio 2012, but I recommend that you use <a href="http://www.visualstudio.com/downloads/download-visual-studio-vs" title="Visual Studio 2013">Visual Studio 2013</a>. The reason is that you can choose to include the <a href="http://developer.windowsphone.com/en-us/downloadsdk" title="Windows Phone 8 SDK">Windows Phone 8 SDK</a> upon installation, and the Windows Store templates come out of the box with VS 2013. The Windows Phone 8 emulator uses Hyper-V, and since we will be creating an app for Windows Store, that means you'll need at least <a href="http://www.microsoft.com/en-us/windows/business/default.aspx" title="Windows 8 Professional">Windows 8 Professional</a> installed on your machine. Once you've got VS 2013 with the Windows Phone 8 SDK installed on your Windows 8 Professional OS, we're ready to get started.</p>
<p><strong>Let's Get Started</strong></p>
<p>Start Visual Studio 2013 and create a new project, choose Windows Phone:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture1.png"><img src="{{ site.baseurl }}/assets/capture1.png?w=300" alt="Capture" width="300" height="213" class="alignnone size-medium wp-image-199" /></a></p>
<p>Right click the solution and choose Add -&gt; New Project... Choose Windows Store:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture21.png"><img src="{{ site.baseurl }}/assets/capture21.png?w=300" alt="Capture2" width="300" height="211" class="alignnone size-medium wp-image-201" /></a></p>
<p>Right click solution again and choose Add -&gt; New Project... Choose Portable Class Library:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture3.png"><img src="{{ site.baseurl }}/assets/capture3.png?w=300" alt="Capture3" width="300" height="209" class="alignnone size-medium wp-image-202" /></a></p>
<p>Make sure the following are checked:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture4.png"><img src="{{ site.baseurl }}/assets/capture4.png?w=300" alt="Capture4" width="300" height="293" class="alignnone size-medium wp-image-203" /></a></p>
<p>Note: You can target other platforms by installing <a href="http://xamarin.com/?gclid=CI65ktum0LsCFYFe3godU08A-g" title="Xamarin">Xamarin</a>, but this is not required in this tutorial.</p>
<p>We will start by setting up our PCL. First, delete <code>Class1.cs</code>. Now right click the PCL project and choose Manage NuGet Packages... Search for "mvvm cross" and install "MvvmCross" through NuGet. Delete the <code>ToDo-MvvmCross</code> folder. Go to <code>FirstViewModel.cs</code> and change "Hello MvvmCross" to "Hello World!":</p>
<p>[code language="csharp"]<br />
public class FirstViewModel : MvxViewModel<br />
{<br />
	private string _hello = &quot;Hello World!&quot;;<br />
        public string Hello<br />
	{<br />
		get { return _hello; }<br />
		set { _hello = value; RaisePropertyChanged(() =&gt; Hello); }<br />
	}<br />
}<br />
[/code]</p>
<p>Take a look at the <code>App.cs</code> class, this is the start up class of our library that registers our view models:</p>
<p>[code language="csharp"]<br />
public class App : Cirrious.MvvmCross.ViewModels.MvxApplication<br />
{<br />
   public override void Initialize()<br />
   {<br />
      CreatableTypes()<br />
         .EndingWith(&quot;Service&quot;)<br />
         .AsInterfaces()<br />
         .RegisterAsLazySingleton();</p>
<p>         RegisterAppStart&lt;ViewModels.FirstViewModel&gt;();<br />
   }<br />
}<br />
[/code]</p>
<p>We've finished setting up our PCL. Now navigate to the Windows Phone project and delete <code>MainPage.xaml</code>. Right click the project and choose Manage NuGet Packages... Search for "mvvm cross" and install "MvvmCross" through NuGet. Delete the <code>ToDo-MvvmCross</code> folder. Now right click References and choose Add Reference... Click on Solution -&gt; Projects and choose the PCL project. Now navigate to the <code>Setup.cs</code> class, this class sets up the data binding to our view model among other things. Change it to look like this:</p>
<p>[code language="csharp"]<br />
public class Setup : MvxPhoneSetup<br />
{<br />
    public Setup(PhoneApplicationFrame rootFrame) : base(rootFrame)<br />
    {<br />
    }</p>
<p>    protected override IMvxApplication CreateApp()<br />
    {<br />
        return new PortableClassLibrary1.App();<br />
    }</p>
<p>    protected override IMvxTrace CreateDebugTrace()<br />
    {<br />
        return new DebugTrace();<br />
    }<br />
}<br />
[/code]</p>
<p>Now we need to make sure that our Phone app uses <code>Setup.cs</code>. To do that, navigate to <code>App.xaml.cs</code> and add this to the end of the constructor:</p>
<p>[code language="csharp"]<br />
var setup = new Setup(RootFrame);<br />
setup.Initialize();<br />
[/code]</p>
<p>Add this field to the class:</p>
<p>[code language="csharp"]<br />
private bool _hasDoneFirstNavigation = false;<br />
[/code]</p>
<p>Now add the following code to the <code>Application_Launching</code> method:</p>
<p>[code language="csharp"]<br />
RootFrame.Navigating += (navigatingSender, navigatingArgs) =&gt;<br />
{<br />
    if (_hasDoneFirstNavigation)<br />
          return;</p>
<p>    navigatingArgs.Cancel = true;<br />
    _hasDoneFirstNavigation = true;<br />
    var appStart = Mvx.Resolve&lt;IMvxAppStart&gt;();<br />
    RootFrame.Dispatcher.BeginInvoke(() =&gt; appStart.Start());<br />
};<br />
[/code]</p>
<p>Now take a look at <code>FirstView.xaml</code> and how it binds to our view model:</p>
<p>[code language="xml"]<br />
&lt;views:MvxPhonePage<br />
    x:Class=&quot;PhoneApp1.Views.FirstView&quot;<br />
    xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;<br />
    xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;<br />
    xmlns:phone=&quot;clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone&quot;<br />
    xmlns:shell=&quot;clr-namespace:Microsoft.Phone.Shell;assembly=Microsoft.Phone&quot;<br />
    xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;<br />
    xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;<br />
    xmlns:views=&quot;clr-namespace:Cirrious.MvvmCross.WindowsPhone.Views;assembly=Cirrious.MvvmCross.WindowsPhone&quot;<br />
    FontFamily=&quot;{StaticResource PhoneFontFamilyNormal}&quot;<br />
    FontSize=&quot;{StaticResource PhoneFontSizeNormal}&quot;<br />
    Foreground=&quot;{StaticResource PhoneForegroundBrush}&quot;<br />
    SupportedOrientations=&quot;Portrait&quot; Orientation=&quot;Portrait&quot;<br />
    mc:Ignorable=&quot;d&quot;<br />
    shell:SystemTray.IsVisible=&quot;True&quot;&gt;</p>
<p>    &lt;!--LayoutRoot is the root grid where all page content is placed--&gt;<br />
    &lt;Grid x:Name=&quot;LayoutRoot&quot; Background=&quot;Transparent&quot;&gt;<br />
        &lt;Grid.RowDefinitions&gt;<br />
            &lt;RowDefinition Height=&quot;Auto&quot;/&gt;<br />
            &lt;RowDefinition Height=&quot;*&quot;/&gt;<br />
        &lt;/Grid.RowDefinitions&gt;</p>
<p>        &lt;!--TitlePanel contains the name of the application and page title--&gt;<br />
        &lt;StackPanel x:Name=&quot;TitlePanel&quot; Grid.Row=&quot;0&quot; Margin=&quot;12,17,0,28&quot;&gt;<br />
            &lt;TextBlock x:Name=&quot;ApplicationTitle&quot; Text=&quot;MY APPLICATION&quot; Style=&quot;{StaticResource PhoneTextNormalStyle}&quot;/&gt;<br />
            &lt;TextBlock x:Name=&quot;PageTitle&quot; Text=&quot;page name&quot; Margin=&quot;9,-7,0,0&quot; Style=&quot;{StaticResource PhoneTextTitle1Style}&quot;/&gt;<br />
        &lt;/StackPanel&gt;</p>
<p>        &lt;!--ContentPanel - place additional content here--&gt;<br />
        &lt;Grid x:Name=&quot;ContentPanel&quot; Grid.Row=&quot;1&quot; Margin=&quot;12,0,12,0&quot;&gt;<br />
		   &lt;StackPanel&gt;<br />
			&lt;TextBox Text=&quot;{Binding Hello, Mode=TwoWay}&quot; /&gt;<br />
			&lt;TextBlock Text=&quot;{Binding Hello}&quot; /&gt;<br />
		   &lt;/StackPanel&gt;<br />
        &lt;/Grid&gt;<br />
    &lt;/Grid&gt;</p>
<p>&lt;/views:MvxPhonePage&gt;<br />
[/code]</p>
<p>That's it! Build and run the Phone project, the emulator should start up:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture5.png"><img src="{{ site.baseurl }}/assets/capture5.png?w=200" alt="Capture5" width="200" height="300" class="alignnone size-medium wp-image-213" /></a></p>
<p>Let's move on to the Windows Store project. Delete <code>MainPage.xaml</code>, right click the project and choose Manage NuGet Packages... Search for "mvvm cross" and install "MvvmCross" through NuGet. Delete the <code>ToDo-MvvmCross</code> folder. Now right click References and choose Add Reference... Click on Solution -&gt; Projects and choose the PCL project. Now navigate to the <code>Setup.cs</code> class, and make sure it looks correct:</p>
<p>[code language="csharp"]<br />
public class Setup : MvxStoreSetup<br />
{<br />
        public Setup(Frame rootFrame) : base(rootFrame)<br />
        {<br />
        }</p>
<p>        protected override IMvxApplication CreateApp()<br />
        {<br />
            return new PortableClassLibrary1.App();<br />
        }</p>
<p>        protected override IMvxTrace CreateDebugTrace()<br />
        {<br />
            return new DebugTrace();<br />
        }<br />
}<br />
[/code]</p>
<p>Navigate to <code>App.xaml.cs</code> and <i>remove</i> the following code from the <code>OnLaunched</code> method:</p>
<p>[code language="csharp"]<br />
if (rootFrame.Content == null)<br />
            {<br />
                // When the navigation stack isn't restored navigate to the first page,<br />
                // configuring the new page by passing required information as a navigation<br />
                // parameter<br />
                rootFrame.Navigate(typeof(MainPage), e.Arguments);<br />
            }<br />
[/code]</p>
<p>Add the following to the end of the <code>OnLaunched</code> method:</p>
<p>[code language="csharp"]<br />
var setup = new Setup(rootFrame);<br />
setup.Initialize();</p>
<p>var start = Mvx.Resolve&lt;IMvxAppStart&gt;();<br />
start.Start();<br />
[/code]</p>
<p>Delete <code>FirstView.xaml</code>. Right click the <code>Views</code> folder and add a new Basic Page, call it "FirstView.xaml":</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture61.png"><img src="{{ site.baseurl }}/assets/capture61.png?w=300" alt="Capture6" width="300" height="207" class="alignnone size-medium wp-image-221" /></a></p>
<p>You'll note that a new folder called <code>Common</code> with helper classes was added to the project. Go to <code>FirstView.xaml.cs</code> and make the class inherit from <code>MvxStorePage</code> instead of <code>Page</code>. Remove the methods <code>OnNavigatedTo</code> and <code>OnNavigatedFrom methods</code>. The class should now look like the following (comments removed):</p>
<p>[code language="csharp"]<br />
public sealed partial class FirstView : MvxStorePage<br />
{<br />
        private NavigationHelper navigationHelper;<br />
        private ObservableDictionary defaultViewModel = new ObservableDictionary();</p>
<p>        public ObservableDictionary DefaultViewModel<br />
        {<br />
            get { return this.defaultViewModel; }<br />
        }</p>
<p>        public NavigationHelper NavigationHelper<br />
        {<br />
            get { return this.navigationHelper; }<br />
        }</p>
<p>        public FirstView()<br />
        {<br />
            this.InitializeComponent();<br />
            this.navigationHelper = new NavigationHelper(this);<br />
            this.navigationHelper.LoadState += navigationHelper_LoadState;<br />
            this.navigationHelper.SaveState += navigationHelper_SaveState;<br />
        }</p>
<p>        private void navigationHelper_LoadState(object sender, LoadStateEventArgs e)<br />
        {<br />
        }</p>
<p>        private void navigationHelper_SaveState(object sender, SaveStateEventArgs e)<br />
        {<br />
        }<br />
}<br />
[/code]</p>
<p>Now navigate to <code>FirstView.xaml</code> and reflect the changes that you made in the code-behind file, so your view looks like the following:</p>
<p>[code language="xml"]<br />
&lt;views:MvxStorePage<br />
    x:Name=&quot;pageRoot&quot;<br />
    x:Class=&quot;App1.Views.FirstView&quot;<br />
    DataContext=&quot;{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}&quot;<br />
    xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;<br />
    xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;<br />
    xmlns:local=&quot;using:App1.Views&quot;<br />
    xmlns:common=&quot;using:App1.Common&quot;<br />
    xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;<br />
    xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;<br />
    xmlns:views=&quot;using:Cirrious.MvvmCross.WindowsStore.Views&quot;<br />
    mc:Ignorable=&quot;d&quot;&gt;</p>
<p>    &lt;Page.Resources&gt;<br />
        &lt;!-- TODO: Delete this line if the key AppName is declared in App.xaml --&gt;<br />
        &lt;x:String x:Key=&quot;AppName&quot;&gt;My Application&lt;/x:String&gt;<br />
    &lt;/Page.Resources&gt;</p>
<p>    &lt;!--<br />
        This grid acts as a root panel for the page that defines two rows:<br />
        * Row 0 contains the back button and page title<br />
        * Row 1 contains the rest of the page layout<br />
    --&gt;<br />
    &lt;Grid Background=&quot;{ThemeResource ApplicationPageBackgroundThemeBrush}&quot;&gt;<br />
        &lt;Grid.ChildrenTransitions&gt;<br />
            &lt;TransitionCollection&gt;<br />
                &lt;EntranceThemeTransition/&gt;<br />
            &lt;/TransitionCollection&gt;<br />
        &lt;/Grid.ChildrenTransitions&gt;<br />
        &lt;Grid.RowDefinitions&gt;<br />
            &lt;RowDefinition Height=&quot;140&quot;/&gt;<br />
            &lt;RowDefinition Height=&quot;Auto&quot;/&gt;<br />
        &lt;/Grid.RowDefinitions&gt;</p>
<p>        &lt;!-- Back button and page title --&gt;<br />
        &lt;Grid&gt;<br />
            &lt;Grid.ColumnDefinitions&gt;<br />
                &lt;ColumnDefinition Width=&quot;120&quot;/&gt;<br />
                &lt;ColumnDefinition Width=&quot;*&quot;/&gt;<br />
            &lt;/Grid.ColumnDefinitions&gt;<br />
            &lt;Button x:Name=&quot;backButton&quot; Margin=&quot;39,59,39,0&quot; Command=&quot;{Binding NavigationHelper.GoBackCommand, ElementName=pageRoot}&quot;<br />
                        Style=&quot;{StaticResource NavigationBackButtonNormalStyle}&quot;<br />
                        VerticalAlignment=&quot;Top&quot;<br />
                        AutomationProperties.Name=&quot;Back&quot;<br />
                        AutomationProperties.AutomationId=&quot;BackButton&quot;<br />
                        AutomationProperties.ItemType=&quot;Navigation Button&quot;/&gt;<br />
            &lt;TextBlock x:Name=&quot;pageTitle&quot; Text=&quot;{StaticResource AppName}&quot; Style=&quot;{StaticResource HeaderTextBlockStyle}&quot; Grid.Column=&quot;1&quot;<br />
                        IsHitTestVisible=&quot;false&quot; TextWrapping=&quot;NoWrap&quot; VerticalAlignment=&quot;Bottom&quot; Margin=&quot;0,0,30,40&quot;/&gt;<br />
        &lt;/Grid&gt;<br />
        &lt;Grid x:Name=&quot;ContentPanel&quot; Grid.Row=&quot;1&quot; Margin=&quot;12,0,12,0&quot;&gt;<br />
            &lt;StackPanel&gt;<br />
                &lt;TextBox Text=&quot;{Binding Hello, Mode=TwoWay}&quot; /&gt;<br />
                &lt;TextBlock Text=&quot;{Binding Hello}&quot; /&gt;<br />
            &lt;/StackPanel&gt;<br />
        &lt;/Grid&gt;<br />
    &lt;/Grid&gt;<br />
&lt;/views:MvxStorePage&gt;<br />
[/code]</p>
<p>Build and run the Store app:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture7.png"><img src="{{ site.baseurl }}/assets/capture7.png?w=300" alt="Capture7" width="300" height="187" class="alignnone size-medium wp-image-224" /></a></p>
<p>And we are done! Both applications now use the logic written in the Portable Class Library, and you only need to write the logic once. Pretty cool! Now you can go ahead and make some awesome cross-platform apps. :)</p>
