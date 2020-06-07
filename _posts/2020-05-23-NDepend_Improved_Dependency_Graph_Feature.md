---
title: "NDepend - Improved Dependency Graph Feature"
published: true
description: "NDepend - Improved Dependency Graph Feature"
tags: ndepend code-quality
cover_image: 
canonical_url: http://niranjankala.in/post/ndepend-improved-dependency-graph-feature
layout: post
---
    
## Introduction

This is another post related to the NDepend tool and Code Quality for .NET Core application follow by [this](http://redirect.viglink.com/?format=go&jsonp=vglnk_159024602325914&key=034153a8f6f990b64f375d12e1cc4572&libId=kajrdihc01000nv1000DA1dfvlkk42xtsu&loc=http%3A%2F%2Fniranjankala.blogspot.com%2F2020%2F05%2Fndepend-improved-dependency-graph.html%3Futm_source%3Ddlvr.it%26utm_medium%3Dlinkedin&v=1&out=http%3A%2F%2Fniranjankala.in%2Fpost%2FNDepend-enabled-all-features-for-NET-Core-20-Visual-Studio-projects&title=Niranjan%20Kala%27s%20blog%3A%20NDepend%20-%20Improved%20Dependency%20Graph%20Feature&txt=this).

NDepend makes .NET code beautiful by measuring quality with metrics, generate diagrams, and enforce decisions with code rules, right in Visual Studio.

I have used this tool for analyzing the .NET Core project and it is a great and intuitive tool to work with. The new version [NDepend](http://redirect.viglink.com/?format=go&jsonp=vglnk_159024600483712&key=034153a8f6f990b64f375d12e1cc4572&libId=kajrdihc01000nv1000DA1dfvlkk42xtsu&loc=http%3A%2F%2Fniranjankala.blogspot.com%2F2020%2F05%2Fndepend-improved-dependency-graph.html%3Futm_source%3Ddlvr.it%26utm_medium%3Dlinkedin&v=1&out=https%3A%2F%2Fwww.ndepend.com%2Fwhatsnew&title=Niranjan%20Kala%27s%20blog%3A%20NDepend%20-%20Improved%20Dependency%20Graph%20Feature&txt=NDepend%26nbsp%3B)  v2020.1.0  is available with the latest features e.g.  Dependency Graph Completely Rebuilt

### What is new with the Dependency Graph
I have also used the Dependency Graph in the previous version of the NDepend. It was much useful to identify the dependency between the different assemblies in the solution.

This time the NDepend team improvise this feature and restructured to analyze large project architecture. Below features make it easy to analyze using the dependency graph.

### New navigation system
you can drag and drop from Visual Studio solution explorer, Expand/Collapse parent elements, Search elements in graphs by name with the search windows, also provided Undo / Redo feature.

### New layout options
- Group-By Assemblies, Namespaces, Types, Clusters Filters to show or hide
Box size proportional to element size, Edge width proportional to the coupling strength
- Color conventions instantly identify caller/callee elements
- Complex graph simplified with Clusters
- Export to SVG vector format or PNG bitmap format.


Let's explorer that how can we generate Dependency Graph by taking an example of the OrchardCore application which contains more than 143 projects. 

Open the application in the Visual Studio and create an NDepend project by attached to the Visual Studio solution.
![1](https://1.bp.blogspot.com/-lZdRCUECU4o/XskqWyWvO5I/AAAAAAAABwU/GAlDPUXdh-88dZLGQ7WPNELrR9UUMf_0wCLcBGAsYHQ/s640/Create%2BNDepend%2BProject.png)
Filter and select the project that you want to analyze.
![2](https://1.bp.blogspot.com/-KZNPlrSH6S0/XskqaYaD6LI/AAAAAAAABwc/vByoTdvOE_0--JtAa1-52ySydChcG3YwgCLcBGAsYHQ/s640/Analyze%2BApplication.png)
Now a dialog box will open with options, View NDepend Dashboard, Show NDepend Interactive Dependency Graph. Click on the "Show NDepend Interactive Dependency Graph" button to see the dependency graph.
![3](https://1.bp.blogspot.com/-yIhx8u1uL7Y/XskqaktLQfI/AAAAAAAABwk/q718oxzr4NwGW8WqzNsfSzHmKGQtTDDWwCLcBGAsYHQ/s640/Select%2BDependency%2BGrapsh.png)

After a few seconds windows will open which is shown in the image below. It has the relationship between the projects and their dependencies.
![4](https://1.bp.blogspot.com/-jfhncsmM3aI/XskqaQ1F1qI/AAAAAAAABwg/m--8HH_4mHU8IMXQvBMcgit7mvl3Nce_ACLcBGAsYHQ/s640/Orchard%2BDependency%2BGraph.png)

You can also zoom in or out of the dependency graph to see a group of objects. See the below screenshot to know the zoom feature. you can move and see the project elements.
![5](https://1.bp.blogspot.com/-Dh4uQz9GtC4/XskqaHVeIdI/AAAAAAAABwY/zvoEmfSloU0wf9yiHE7_9m_aErhEDmrGgCLcBGAsYHQ/s640/Dependency%2BGraph%2B-%2BZoom.png)
## Conclusion
I genuinely think NDepend is very powerful and very easy to use for analyzing application architecture. The NDepend team and Patrick are very active and we can expect a lot of good improvements in future versions which help to create quality software.
