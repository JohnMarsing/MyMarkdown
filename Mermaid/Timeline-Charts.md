# Timeline Charts
- [Matt Eland's Timeline Chart example](https://newdevsguide.com/2023/04/16/mermaid-timeline-charts/)

```mermaid
timeline
    title Major .NET Releases
    section .NET Framework
        2000 - 2005 
             : .NET Framework 1.0
             : .NET Framework 1.0 SP1
             : .NET Framework 1.0 SP2
             : .NET Framework 1.1
             : .NET Framework 1.0 SP3
             : .NET Framework 2.0
        2006 - 2009 
             : .NET Framework 3.0
             : .NET Framework 3.5 
             : .NET Framework 2.0 SP 1 
             : .NET Framework 3.0 SP 1 
             : .NET Framework 2.0 SP 2 
             : .NET Framework 3.0 SP 2 
             : .NET Framework 3.5 SP 1
        2010 - 2015 
             : .NET Framework 4.0
             : .NET Framework 4.5
             : .NET Framework 4.5.1
             : .NET Framework 4.5.2
             : .NET Framework 4.6
             : .NET Framework 4.6.1
    section .NET Core
        2016 - 2017 
             : .NET Core 1.0
             : .NET Core 1.1
             : .NET Framework 4.6.2
             : .NET Core 2.0
             : .NET Framework 4.7
             : .NET Framework 4.7.1
        2018 - 2019 
             : .NET Core 2.1
             : .NET Core 2.2
             : .NET Framework 4.7.2             
             : .NET Core 3.0
             : .NET Core 3.1
             : .NET Framework 4.8
    section Modern .NET
        2020 : .NET 5
        2021 : .NET 6
        2022 : .NET 7
             : .NET Framework 4.8.1
```

[Gantt Carts](https://newdevsguide.com/2023/04/14/mermaid-gantt-chart/) can be great for detailed task analysis, but sometimes you just want to look at a high-level view of what's going on in a time period. Mermaid.js gives us **Timeline Charts** to help with that.

### the process of building this chart, step by step.

JKM: how would I do this to express [MHB Timeline](https://myhebrewbible.com/List/TimeLine)

1. Defining Time Ranges
> I would need to figure out how to do this differentiating between BC and AD

2. Adding Timeline Entries
> [MHB Favorite Verses](http://www.myhebrewbible.com/FavoriteVerses) 

3. Adding Sections to our Timeline Chart
You can group together multiple columns into a section to help convey meaning or relationships.

> Bc,Ad, before divorce, during divorce, after redemption 

4. Adding a Title

#### Marsing’s Insight 

A brief introduction why my take on this whole thing is unique and hopefully powerful. Point to page one of 
my website as an example to get better understanding. A great source for a timeline would be the table of 
contents I created for my favorites list source 

> Source [Exegesis Timeline #796](https://myhebrewbible.com/Article/796/Exegesis-Timeline)
