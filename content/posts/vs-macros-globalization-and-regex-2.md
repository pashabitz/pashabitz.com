---
title: 'VS Macros, Globalization and Regex'
date: Sun, 27 Aug 2006 11:52:36 +0000
draft: false
tags: ['Misc']
---

**Summary**: In this short article I will describe the creation of a Visual Studio.NET 2005 macro that speeds up the process of adding localizable controls to ASP.NET web pages, briefly touching on the subjects of ASP.NET localization, regular expressions and world peace.

Not really world peace.

I will assume you are familiar with writing ASP.NET pages and working with regular expressions.

**Intro on ASP.NET Globalization**

Globalization is the process of building an application in a way that it's GUI can appear in diferrent languages and cultural formats. ASP.NET 2.0 has some mechanisms that simplify building globalized apps with (relatively) little hassle.

One of the widest used mechanisms is called **implicit localization expressions**. It works by placing a special attribute, **meta:resourcekey** on an ASP.NET control in the page:

<asp:label id="Header" runat="server" **meta:resourcekey="Header"**\></asp:label>

Now, you can create resource files in the App\_LocalResources directory of the web project. The resource files must adhere to the naming convention:

_\[LocalizedWebFormName\].resx_

So if the above asp:label is in a file named **Default.aspx**, the resource file will be named **Default.aspx.resx**.

In the resource file you can now add resources with names that match the values in the meta:resourcekey attributes of the localized controls and the property names of the controls that you want to localize(such as the Text property of an asp:label control).

For example:

![](http://www.writely.com/File.aspx?id=dcfnfqdw_2cqp5gv)

When you run the page, ASP.NET will automatically populate the properties of the localized control with the values from the resource file.

Now for the localization part - you can supply resource files for every language and culture you wish to support by creating a resource file named in the following convention:

_\[LocalizedWebFormName\].\[Language Code\].resx_

For example:

Default.aspx.en.resx

Default.aspx.fr.resx

Default.aspx.he.resx

The beauty is that ASP.NET will load properties with values from the resource file that matches the culture of the current thread, based on the current culture of the requesting user's machine. So when a user from Israel will request the page she will get the Hebrew values automatically.

**Very Cool, But There's a Rub**

Now you can build globalized web pages by dragging controls, adding the meta:resourcekey attribute and adding the needed property resources to the resource file.

One problem is that creating even simple pages that contain many controls can become tedious, because now you have to spend much more time on every control you drag to the web page.

How can we automate the process of adding the extra attributes and resources?

**VS Macros 101**

Visual Studio.NET 2005 contains two main customization and extension mechanisms: Macros and Add-Ins. Generally speaking Macros are less powerful but easier to create. Add-Ins are more powerful and are a bit more complicated to create.

VS Macros are actually Visual Basic for Applications code (that's right, no VB.NET and definitely no C#) intended primary for automating repetitive tasks in  Visual Studio.

The easiest way to create a macro is by using the Record Macro command:

 ![](http://www.writely.com/File.aspx?id=dcfnfqdw_3f7m534)

Recording a macro in this way actually creates VBA code behind the scenes. You then can edit that code and also create new macros programmatically using the Macros IDE. This code can access many parts of the Visual Studio environment such as the files ad the code inside them, via different classes in the EnvDTE namespace.

Good places to start learning about how you can access the various parts of VS are:

1\. [Automation and Extensibility reference on MSDN](http://msdn2.microsoft.com/en-us/library/1xt0ezx9.aspx "Automation and Extensibility reference on MSDN").

2\. Opening the Macros IDE and reading the code in the Samples project.

3\. Recording macros and going through the code that VS creates automatically.

**Back to the Problem, Macro For Localized Controls**

Lets draw an outline for one possible solution:

The user will create the controls normally, then select in the editor all the controls that need to be localized and run the macro. The macro will go through the controls that are found within the selection, creating a meta:resourcekey attribute for each one (the attribute value will be the same as the value of the **id** attribute) and also creating a resource for each control in the appropriate resource file.

**Writing the Macro - Adding Localization Attributes to the Web Page**

We'll start out by adding a new public method to the macros module:

Sub LocalizeControls()

End Sub

The procedure is public by default. Private procedures will not be accessible from the IDE and cannot be used as macro entry-points.

We will use regular expressions to find all the ASP.NET controls inside the selection and to append the meta:resourcekey attributes. The following code section does just that:

Const ControlsMatchingPattern = "(?<tag><asp\[^>\]\*id=" & Chr(34) & "(?<id>\[^" & Chr(34) & "\]\*)" & Chr(34) & "\[^>\]\*text=" & Chr(34) & "(?<text>\[^" & Chr(34) & "\]\*)" & Chr(34) & "\[^>\]\*)>"

Function AddMetaResourceKeyToControls(ByVal ControlsHtml As  String)

> Dim ControlsRegexExpression As  New Regex(ControlsMatchingPattern, RegexOptions.IgnoreCase)
> 
> Dim ReplacementPattern As  String = "${tag} meta:resourcekey=" & Chr(34) & "${id}" & Chr(34) & ">"
> 
> AddMetaResourceKeyToControls = ControlsRegexExpression.Replace(ControlsHtml, ReplacementPattern)

End  Function

Sub LocalizeControls()

> Dim CurrentSelection As TextSelection = DTE.ActiveDocument.Selection
> 
> Dim ToReplace As  String = CurrentSelection.Text
> 
> Dim Replaced As  String = AddMetaResourceKeyToControls(ToReplace)
> 
> CurrentSelection.Insert(Replaced)

End  Sub

The _AddMetaResourceKeyToControls_ function receives some text and finds all the occurrences of ASP.NET controls in it. The simplest way to do this is using regular expressions.

**Tip:** I suggest you use a tool like [Expresso](http://www.codeproject.com/dotnet/expresso.asp "Expresso") for building regular expressions. It simplifies remembering the syntax of regular expressions. It also allows you to test your expressions against some input text and is .NET specific.

In this case we want to find everything that looks like this:

<asp:\[something\] >

Note that our code will only handle ASP.NET built-in controls for sake of simplicity.

The basic regular expression to perform this is:

<asp:\[^>\]\*>

Let's further limit ourselves to controls that contain an id attribute and a text attribute:

<asp:\[^>\]\***id="\[^"\]\*"\[^>\]\*text="\[^"\]\*"**\[^>\]\*>

To use the replacement feature of the .NET Regex class we need to use a feature called **groups**. Groups allow us to temporarily store certain parts of our matches and use them in the replacement pattern. In our case we want to make the following kind of transformation:

<asp:label id="Header" text="Hello World">    **\->**    <asp:label id="Header" text="Hello World" meta:resourcekey="Header">

Let's store the whole control's opening tag in a group:

**(?<tag>**<asp:\[^>\]\*id="\[^"\]\*"\[^>\]\*text="\[^"\]\*"\[^>\]\***)**\>

So that we can later use it in the replacement. Note the placement of the closing parenthesis. That's because I will need to plant the new attribute before the closing of the ASP.NET control tag.

We also need to extract the value of the id and text attributes for later use:

(?<tag><asp:\[^>\]\*id="**(?<id>**\[^"\]\***)**"\[^>\]\*text="**(?<text>**\[^"\]\***)**"\[^>\]\*)>

Now, for the replacement pattern. It's rather simple, we take the **tag** group from the match and append the new localization attribute:

${tag} meta:resourcekey="${id}">

With the regex patterns set, we can use the _Replace_ method of the Regex class to append the localization attributes to our controls. Note the use of the _IgnoreCase_ flag when creating the Regex class. We want to catch controls that have id or ID attributes.

Replacing the actual code in the document is a matter of calling the _Insert_ method of the _TextSelection_ object in the _ActiveDocument_._Selection_ property. That's the current selection made by the user.

**Adding Resources**

Now let's automatically add the needed resources to the resource file. To find the resource file:

Function GetLocalResourcesDir()

> Dim CurrentProject As EnvDTE.Project = DTE.ActiveDocument.ProjectItem.ContainingProject
> 
> For  Each Item As ProjectItem In CurrentProject.ProjectItems
> 
> If (Item.Name = "App\_LocalResources") Then
> 
> > GetLocalResourcesDir = Item
> > 
> > Exit  Function
> 
> End  If
> 
> Next
> 
> GetLocalResourcesDir = Nothing

End  Function

Function GetLocalResourceFileForCurrentDocument()

> Dim LocalResourcesDirItem As ProjectItem = GetLocalResourcesDir()
> 
> If (LocalResourcesDirItem Is  Nothing) Then
> 
> > MsgBox("App\_LocalResources directory not found. Will not add resources.")
> > 
> > Exit  Function
> 
> End  If
> 
> For  Each Item As ProjectItem In LocalResourcesDirItem.ProjectItems
> 
> > If (Item.Name = DTE.ActiveDocument.Name & ".resx") Then
> > 
> > > GetLocalResourceFileForCurrentDocument = Item
> > > 
> > > Exit  Function
> > 
> > End  If
> 
> Next
> 
> MsgBox("Default resource file for current document not found. Will not add resources.")
> 
> GetLocalResourceFileForCurrentDocument = Nothing

End  Function

What I'm basically doing here is looking for a resource file named like the currently edited document, with an resx extension, inside the ASP.NET local resources folder. If the item is not found I show an error message to the user. This code can be modified so that the resource file is created if it does not exist yet.

I leave that as an exercise to the reader.

(Don't you just love to read that sentence? I do.)

Visual Studio.NET provides two views for resource files: **designer** and **code**:

 ![](http://www.writely.com/File.aspx?id=dcfnfqdw_4c5g8mr)

In designer view we get a nice grid to edit the file, in code view we actually see the text of the resource file, which suprisingly is just XML. In order to add the needed resources, we will manipulate the file as text. So, after locating the resource file, we will open it in code view and make it the active document:

Private  Sub OpenResourceFileInTextViewAndActivate(ByVal ResourceFileToOpen As ProjectItem)

> If (ResourceFileToOpen.IsOpen) Then
> 
> > ResourceFileToOpen.Document.Close(vsSaveChanges.vsSaveChangesNo)
> 
> End  If
> 
> ResourceFileToOpen.Open(Constants.vsViewKindTextView)
> 
> ResourceFileToOpen.Document.Activate()

End  Sub

Now we are ready to add the new resources to the resource file:

Function GetMatchingControls(ByVal ControlsHtml As  String)

> Dim Matches As MatchCollection
> 
> Dim ControlsRegexExpression As  New Regex(ControlsMatchingPattern, RegexOptions.IgnoreCase)
> 
> GetMatchingControls = ControlsRegexExpression.Matches(ControlsHtml)

End  Function

Private  Sub AddResourcesXmlAtSelection(ByVal Matches As MatchCollection, ByVal AddLocation As TextSelection)

> For  Each M As Match In Matches
> 
> > AddLocation.Insert("<data name=" & Chr(34) & M.Groups.Item("id").Value & ".Text" & Chr(34) & " xml:space=" & Chr(34) & "preserve" & Chr(34) & ">" & Chr(10))
> > 
> > AddLocation.Insert("<value>" & M.Groups.Item("text").Value & "</value>" & Chr(10))
> > 
> > AddLocation.Insert("</data>" & Chr(10))
> 
> Next

End  Sub

Private  Sub AddResourcesToResourceFile(ByVal ControlsHtml As  String)

> Dim ResourceFileForCurrentDocument As ProjectItem = GetLocalResourceFileForCurrentDocument()
> 
> If (ResourceFileForCurrentDocument Is  Nothing) Then
> 
> > Exit  Sub
> 
> End  If
> 
> OpenResourceFileInTextViewAndActivate(ResourceFileForCurrentDocument)
> 
> Dim ResourceFileSelection As TextSelection = DTE.ActiveDocument.Selection
> 
> ResourceFileSelection.EndOfDocument()
> 
> ResourceFileSelection.StartOfLine()
> 
> AddResourcesXmlAtSelection(GetMatchingControls(ControlsHtml), ResourceFileSelection)

End  Sub

The XML of the resource file looks kind of like this:

<?xml version="1.0" encoding="utf-8"?>  
<root>  
  <xsd:schema id="root" xmlns="" xmlns:xsd="[http://www.w3.org/2001/XMLSchema](http://www.w3.org/2001/XMLSchema)" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
 ...  
  </xsd:schema>  
 ...  
 ...  
  <data name="SomeControl.Text" xml:space="preserve">  
    <value>Control Text</value>  
  </data>  
</root>

We want to append additional data tags at the end of the file. In order to do that we need to navigate the TextSelection object to the beginning of the last line:

Dim ResourceFileSelection As TextSelection = DTE.ActiveDocument.Selection

ResourceFileSelection.EndOfDocument()

ResourceFileSelection.StartOfLine()

Now we can use the Regex class again to loop through the controls found in the original user selection and append XML to the resource file, using the **id** and **text** groups from the regex pattern. That is done in the _AddResourcesXmlAtSelection_ method.

What is left is just to add the call to it in our entry-point method _LocalizeControls_.

**Deploying Macros**

If you want to use your macros across solutions and share them with other team members, you should create a new Macro Project using the Macro Explorer window in VS. Drag the module with your macro code to that project and save the project. The default saving location for macro projects in VS.NET 2005 is **Visual Studio 2005ProjectsVSMacros80** under the **My Documents** folder. All the macro project code is saved in one file with a vsmacros extension. You can easily share this file with other people.

To use a macro that resides in some macro project you need to load the macro project using the Macro Explorer. Once you have loaded a macro project it will be available in VS on that machine for all solutions until you manually unload it.