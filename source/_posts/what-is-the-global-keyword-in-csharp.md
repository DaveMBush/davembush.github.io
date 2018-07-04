---
title: What is the global keyword in CSharp?
tags:
  - 'c#'
  - global
url: 708.html
id: 708
categories:
  - 'c#'
date: 2008-12-29 07:01:15
---

During the Christmas break, I received the following question: What does C# global keyword actually do? Code example, from table adapter code:

\[global::System.CodeDom.Compiler.GeneratedCodeAttribute("System.Data.Design.TypedDataSetGenerator", "2.0.0.0")\]
\[global::System.Serializable()\]
\[global::System.ComponentModel.DesignerCategoryAttribute("code")\]
\[global::System.ComponentModel.ToolboxItem(true)\]
\[global::System.Xml.Serialization.XmlSchemaProviderAttribute("GetTypedDataSetSchema")\]
\[global::System.Xml.Serialization.XmlRootAttribute("AutoTwitDataSet")\]
\[global::System.ComponentModel.Design.HelpKeywordAttribute("vs.data.DataSet")\]
public partial class AutoTwitDataSet : global::System.Data.DataSet {

[](//11011.net/software/vspaste)The global keyword tells the compiler to start looking for the namespace or class starting from the root.  You'll see it in system-generated code so that the code always works.  That way if you have a namespace right under your current namespace that is the same as the top level namespace the code is trying to access, there won't be a conflict. For example, say you have namespace A and namespace B and namespace B.A if I write code in namespace B.A that needs to reference a class in namespace A, without global:: I have no way of getting to it.  If I reference A.classname, the compiler will look for classname in B.A.  With global:: I can tell it to look for classname in global::A.classname and it will find classname in the proper location. This keyword snuck in during version 2.0.  Since most of us don't need it, most of us don't even know it exists.  I didn't until this past weekend.