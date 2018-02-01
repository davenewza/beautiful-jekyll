---
layout: post
title: Monitoring vital metrics in Azure
tags: [azure]
---

It's no surprise that server monitoring is an important part of ensuring a healthly production environment. It wouldn't be easy to spot a nasty memory leak or to realise unusual server load, just to name a couple of examples.

An approach that I have found very effective in identifying potential issues on our hosted applications is to set up a dashboard to monitor just the most vital of metrics and over various periods of time. I don't always know or remember what a "typical" value might be for some metric and so, for that reason, it is useful to show how they have behaved over the shorter and longer periods. I have found 1-hour, 24-hour and 7-day intervals to be a handy distribution.

The snippet below demonstates how I organise these metrics - the metric/counter 'type' over the x-axis and the measurement period on the y-axis.

![Azure Vital Metrics Dashboard](2018-02-azure-vital-metrics-dashboard\azure-vital-metrics-dashboard.gif)

 It's nice being able to run your eye vertically down the screen to compare results across these metrics.