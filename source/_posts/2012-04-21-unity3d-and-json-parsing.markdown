---
layout: post
title: "Unity3D and JSON parsing"
date: 2011-04-21 21:54
comments: true
categories: [JSON, Software Development, Unity3D]
---
Today I was working on a Unity3D web player project that requires pulling JSON from a remote server. I needed a way to parse the JSON string. After working with multiple JSON libraries (Jayrock being the main one) I found that LitJson worked well with in Unity once you had the DLL.
I was unable to get Jayrock to work with Unity's web player. Turns out that Unity desktop builds use a subset of the Mono SDK, which can be changed to use the full SDK. However, the web player can not be changed, limiting access to System.web which is needed by Jayrock.

While running ./configure it would output the following error message: 
> configure: error: Please install mono version 1.1.17 or later

I did confirm my mono version:
> Mono JIT compiler version 2.6.7 

After using the Googles for a bit ran accross a post a link to download a zip file that had the DLL already built: Download [litjson-0.5.0-bin.zip (224.2 KB)](http://sourceforge.net/projects/litjson/files/litjson/0.5.0/litjson-0.5.0-bin.zip/download)

With some code changes JsonMapper seems to be working and I'm able to parse the JSON.
> List<LessonProgress> listOfLessonProgress = JsonMapper.ToObject<List<LessonProgress>>(www.text);

**What I was working with:**

Unity3D version: 3.3.0f4

MonoDevelop version: 2.4.2

OS: OSX (10.6.7)
