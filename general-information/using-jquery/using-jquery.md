---
title: Using jQuery
page_title: Using jQuery | UI for ASP.NET AJAX Documentation
description: Using jQuery
slug: introduction/radcontrols-for-asp.net-ajax-fundamentals/using-jquery/using-jquery
tags: using,jquery
published: True
position: 1
---

# Using jQuery



This help articles shows how to include and use embedded and external jQuery with Telerik UI for ASP.NET AJAX suiteand lists the jQuery version history used by the controls.

1. [Including jQuery](#including-jquery)

1. [Including external jQuery](#including-external-jquery)

1. [Using jQuery](#using-jquery)

1. [jQuery Version History in Telerik UI Controls](#jquery-version-history-in-telerik-ui-controls)

1. [Telerik UI Controls Using jQuery](#telerik-ui-controls-using-jquery)

## Including jQuery

If you have any of the controls listed in the [Telerik UI Controls Using jQuery](#telerik-ui-controls-using-jquery)section (with the specified version or newer) on your page then the jQuery __is already included__ and you can omit this paragraph. If not - follow these steps:

1. Add a ScriptReference pointing to __Core.js__ as we use aslightly customized version of jQuery which depends on Core.js.

We have not modified the implementation of jQuery in any way. We have justappended a few more lines of JavaScript at the end of the file in order toavoid any version conflict and to include a few useful jQuery plugins.

````ASPNET
	<asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.Core.js" /> 
````



2. Add a ScriptReference pointing to __jQuery.js__

````ASPNET
	<asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.jQuery.js" /> 
````



Here is how your RadScriptManager (or ScriptManager) should look likein the end:

````ASPNET
	<telerik:RadScriptManager runat="server" ID="RadScriptManager1" >
	   <Scripts>
	       <asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.Core.js" />
	       <asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.jQuery.js" />
	   </Scripts>
	</telerik:RadScriptManager> 
````





## Including external jQuery

You can disable the embedded jQuery library and include an external one. This lets you use your own version for the __$telerik.$__ variable, which exposes the embedded jQuery library.

>note Embedding a custom jQuery is optional and you can do it if you want to utilize only one version throughout your site instead of having the version that comes with the Telerik controls and an additional one. You should be safe if you reference more versions, because the jQuery in our controls is accessed via a separate alias ( __$telerik.$__ ), the purpose of which is to avoid conflicts.
>


You can disable the jQuery scripts our controls bring by default via the following steps:

1. Load the custom version of jQuery you want to use. At this point you have the jQuery embedded in the Telerik controls and the custom one that is loaded viathe following code. Proceed to the next step if you want to replace the embedded version with the custom one.

````ASPNET
	<head>
	    <script src="http://code.jquery.com/jquery-1.9.1.min.js"></script>
	    ...
	</head>
````



1. Disable the embedded jQuery library as described in [Disabling the embedded jQuery](43f5f2f8-9c0f-4982-95db-fdafa058c347) help article.

1. Configure the ScriptReferences in __RadScriptManager__ as shown in following sample. The script that integrates the external jQuery library in our client-side library is located in the file __jQueryExternal.js__.

````ASPNET
	<telerik:RadScriptManager runat="server" ID="RadScriptManager2" EnableEmbeddedjQuery="false">
	    <Scripts>
	        <asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.Core.js" />
	        <asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.jQueryExternal.js" />
	        <asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.jQueryPlugins.js" />
	    </Scripts>
	</telerik:RadScriptManager>
````



>important The Telerik controls depend on the specific version of jQuery they are tested against (above you can check the version that is used in each release of UI for ASP.NET AJAX). It is possible that using an older version of jQuery or a version, greater than the latestone from the[jQuery Version History in Telerik UI Controls](#jquery-version-history-in-telerik-ui-controls)section, will break the controls.
>


## Using jQuery

After including the jQuery file you can start using it. There is a trickthough - the jQuery object is available as __$telerik.$__ insteadof the default __$__ or jQuery aliases. This is so to avoidcompatibility issues with applications which already use (other versions of)jQuery. For more info you can check the [documentationof the noConflict	](http://docs.jquery.com/Core/jQuery.noConflict) method.

Fortunately there are easy workarounds to enable back the__$ alias.__ Choose one of the options below:



1. Using a global variable

````JavaScript
	<script type="text/javascript">
	    window.$ = $telerik.$;
	</script> 
````





2.Using a self-calling anonymous function (__Example 1__):

````JavaScript
	<script type="text/javascript">
		(function ($) {
			$(document).ready(function () {
				alert("Hooray! The document is ready!");
			}
	   );
		})($telerik.$);
	</script> 
````





3. Using the __$telerik.$__ alias:

````JavaScript
	<script type="text/javascript">
		$telerik.$(document).ready(function () {
			alert("Document is ready");
		});
	</script> 
````





4. Include a script reference to the__Telerik.Web.UI.Common.jQueryInclude.js__:

````JavaScript
	<telerik:RadScriptManager ID="RadScriptManager1" runat="server">
	   <Scripts>
	   ......
	          <asp:ScriptReference Assembly="Telerik.Web.UI" Name="Telerik.Web.UI.Common.jQueryInclude.js" />
	   </Scripts>
	</telerik:RadScriptManager> 
````





>caution Note that loading jQueryInclude.js in a script manager will take the global jQuery object and any (external) jQuery loaded later won’t be initialized.
>


Use any of the approaches above.

__Example 1__: Using a self-calling anonymous function to reference jQuery instance.

````ASPNET
	<telerik:RadScriptManager runat="server" ID="RadScriptManager1">
	</telerik:RadScriptManager>
	<telerik:RadComboBox runat="server" ID="RadComboBox1" Width="300px" Skin="Telerik">
	   <Items>
	       <telerik:RadComboBoxItem Text="ASP.NET AJAX UI Controls" />
	       <telerik:RadComboBoxItem Text="WinForms UI Controls" />
	       <telerik:RadComboBoxItem Text="WPF UI Controls" />
	       <telerik:RadComboBoxItem Text="Silverlight UI Controls" />
	       <telerik:RadComboBoxItem Text="Telerik Reporting" />
	       <telerik:RadComboBoxItem Text="Telerik OpenAccess ORM" />
	       <telerik:RadComboBoxItem Text="Telerik Sitefinity CMS" />
	   </Items>
	</telerik:RadComboBox>
	<script type="text/javascript">
		(function ($) {
			$(document).ready(function () {
				$('.rcbItem')
	           .mouseover(function () {
	           	$(this).stop().animate({ paddingLeft: "30px" }, { duration: 300 });
	           })
	           .mouseout(function () {
	           	$(this).stop().animate({ paddingLeft: "4px" }, { duration: 300 });
	           });
			});
		})($telerik.$);
	</script> 
````



## jQuery Version History in Telerik UI Controls

Telerik UI for ASP.NET AJAX Q3 2014 - present are using jQuery version 1.11.1

Telerik UI for ASP.NET AJAX Q1 2013 SP1 - Q2 2014 SP1 are using jQuery version 1.9.1

Telerik UI for ASP.NET AJAX Q1 2012 SP1 - Q1 2013 are using jQuery version 1.7.2

Telerik UI for ASP.NET AJAX Q3 2011 SP1 / Q1 2012 are using jQuery version 1.7.1

Telerik UI for ASP.NET AJAX Q2 2011 SP1 / Q3 2011 are using jQuery version 1.6.4

Telerik UI for ASP.NET AJAX Q2 2011 are using jQuery version 1.6.2

Telerik UI for ASP.NET AJAX Q1 2011 SP1 / SP2 are using jQuery version 1.5.2

Telerik UI for ASP.NET AJAX Q1 2011 are using jQuery version 1.5.1

Telerik UI for ASP.NET AJAX Q3 2010 SP1 / SP2 are using jQuery version 1.4.4

Telerik UI for ASP.NET AJAX Q3 2010 are using jQuery version 1.4.3

Telerik UI for ASP.NET AJAX Q1 / Q2 2010 are using jQuery version 1.4.2

Telerik UI for ASP.NET AJAX Q1 / Q2 2009 are using jQuery version 1.3.2

## Telerik UI Controls Using jQuery

As of Q3 2008, the Telerik.Web.UI assembly includes the jQuery JavaScript library. __Table 1__ shows a list of Telerik controls that are using jQuery.




>caption Table 1: Telerik UI controls which use jQuery.

|  __Control name__  |  __Version__  |
| ------ | ------ |
|RadAjaxLoadingPanel|Q3 2009|
|RadButton|Q3 2010|
|RadCalendar|Q3 2009|
|RadComboBox|Q1 2009|
|RadContextMenu|Q1 2009|
|RadDatePicker|Q3 2009|
|RadDateTimePicker|Q3 2009|
|RadDock|Q1 2009|
|RadEditor|Q3 2009|
|RadFileExplorer|Q3 2009|
|RadGrid (when using declarative client-side binding or column animations)|Q3 2008 -[ declarative client-side binding ](http://demos.telerik.com/aspnet-ajax/grid/examples/client/declarativedatabinding/defaultcs.aspx)Q2 2010 -[ column animations ](http://demos.telerik.com/aspnet-ajax/grid/examples/client/columnanimation/defaultcs.aspx)|
|RadInputManager|Q1 2009|
|RadImageEditor|Q2 2011|
|RadListView|Q1 2012|
|RadMenu|Q1 2009|
|RadMonthYearPicker|Q1 2011|
|RadNotification|Q2 2011|
|RadOrgChart|Q3 2011|
|RadPanelBar|Q1 2009|
|RadRibbonBar|Q1 2011|
|RadRating|Q1 2011|
|RadRotator|Q1 2009|
|RadScheduler|Q3 2008|
|RadSlider|Q1 2009|
|RadSocialShare|Q3 2011|
|RadSpell|Q1 2010|
|RadSplitter|Q1 2009|
|RadTagCloud|Q2 2010|
|RadTicker|Q1 2009|
|RadTileList|Q2 2013|
|RadTimePicker|Q3 2009|
|RadToolTip|Q1 2009|
|RadTreeView|Q3 2008|
|RadWindow|Q1 2009|
|RadXmlHttpPanel|Q3 2010|



# See Also

 * [What is jQuery?]({%slug introduction/radcontrols-for-asp.net-ajax-fundamentals/using-jquery/what-is-jquery?%})

 * [jQuery Intellisense]({%slug introduction/radcontrols-for-asp.net-ajax-fundamentals/using-jquery/jquery-intellisense%})[](43f5f2f8-9c0f-4982-95db-fdafa058c347)