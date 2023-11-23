# WpfBlazorSample
dotnet sdkでwpfでblazorのWindowsアプリを作成するサンプル

## プロジェクト作成
```
dotnet new wpf -o WpfBlazorSample
```
## 依存パッケージの追加
```
dotnet add package Microsoft.AspNetCore.Components.WebView.Wpf
```

## 変更
```
diff --git a/App.xaml.cs b/App.xaml.cs
index be0c3fa..30bda9c 100644
--- a/App.xaml.cs
+++ b/App.xaml.cs
@@ -9,5 +9,9 @@ namespace WpfBlazorSample;
 /// </summary>
 public partial class App : Application
 {
+       protected override void OnStartup(StartupEventArgs e)
+       {
+               base.OnStartup(e);
+       }
 }

diff --git a/MainWindow.xaml b/MainWindow.xaml
index b1d43ec..5645f2e 100644
--- a/MainWindow.xaml
+++ b/MainWindow.xaml
@@ -3,10 +3,15 @@
         xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
         xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
         xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
+        xmlns:blazor="clr-namespace:Microsoft.AspNetCore.Components.WebView.Wpf;assembly=Microsoft.AspNetCore.Components.WebView.Wpf"
         xmlns:local="clr-namespace:WpfBlazorSample"
         mc:Ignorable="d"
         Title="MainWindow" Height="450" Width="800">
     <Grid>
-
+        <blazor:BlazorWebView HostPage="wwwroot\index.html" Services="{DynamicResource services}">
+            <blazor:BlazorWebView.RootComponents>
+                <blazor:RootComponent Selector="#app" ComponentType="{x:Type local:Counter}" />
+            </blazor:BlazorWebView.RootComponents>
+        </blazor:BlazorWebView>
     </Grid>
 </Window>
diff --git a/MainWindow.xaml.cs b/MainWindow.xaml.cs
index c178a24..14694d3 100644
--- a/MainWindow.xaml.cs
+++ b/MainWindow.xaml.cs
@@ -1,4 +1,8 @@
-﻿using System.Text;
+﻿using System;
+using System.Collections.Generic;
+using System.Linq;
+using System.Text;
+using System.Threading.Tasks;
 using System.Windows;
 using System.Windows.Controls;
 using System.Windows.Data;
@@ -8,9 +12,10 @@ using System.Windows.Media;
 using System.Windows.Media.Imaging;
 using System.Windows.Navigation;
 using System.Windows.Shapes;
+using Microsoft.Extensions.DependencyInjection;

-namespace WpfBlazorSample;
-
+namespace BlazorWpfSample
+{
        /// <summary>
        /// Interaction logic for MainWindow.xaml
        /// </summary>
@@ -18,6 +23,9 @@ public partial class MainWindow : Window
        {
                public MainWindow()
                {
-        InitializeComponent();
+                       var serviceCollection = new ServiceCollection();
+                       serviceCollection.AddWpfBlazorWebView();
+                       Resources.Add("services", serviceCollection.BuildServiceProvider());
+               }
        }
 }
\ No newline at end of file
diff --git a/WpfBlazorSample.csproj b/WpfBlazorSample.csproj
index 34a8ee9..31f6d26 100644
--- a/WpfBlazorSample.csproj
+++ b/WpfBlazorSample.csproj
@@ -1,4 +1,4 @@
-﻿<Project Sdk="Microsoft.NET.Sdk">
+﻿<Project Sdk="Microsoft.NET.Sdk.Razor">

   <PropertyGroup>
     <OutputType>WinExe</OutputType>
@@ -6,6 +6,14 @@
     <Nullable>enable</Nullable>
     <ImplicitUsings>enable</ImplicitUsings>
     <UseWPF>true</UseWPF>
+    <PublishSingleFile>true</PublishSingleFile>
+    <SelfContained>true</SelfContained>
+    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
+    <EnableCompressionInSingleFile>true</EnableCompressionInSingleFile>
   </PropertyGroup>

+  <ItemGroup>
+    <PackageReference Include="Microsoft.AspNetCore.Components.WebView.Wpf" Version="8.0.3" />
+  </ItemGroup>
+
 </Project>
```
