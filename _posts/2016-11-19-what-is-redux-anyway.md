---
id: 46
title: 'What&#8217;s Redux Anyway?'
date: 2016-11-19T14:07:04+00:00
author: Timothy McLane
layout: post
guid: http://www.timothy.codes/?p=46
permalink: /2016/11/19/what-is-redux-anyway/
dsq_thread_id:
  - "5505433625"
categories:
  - frameworks + libraries
  - react
  - redux
tags:
  - react
  - redux
  - talk
---
My team at iModules has been using React for a while now, somewhere around 12-18 months. The team started using React with Flux, which, while useful, came with a whole set of issues and made it incredibly confusing to get up-and-running for new team members. We tend to swap members between teams about once a quarter, which necessitates paradigms that are relatively easy to pick up and very efficient once learnt.

Since I joined the team about five months ago, we started using Redux on a new project. (This shift was mostly at my urging, because I love everything [FRP](https://en.wikipedia.org/wiki/Functional_reactive_programming), and I&#8217;m thankful the team was receptive to it.) In that time, we&#8217;ve learned a lot and have been absolutely thrilled with the paradigm that Redux introduces to front-end development with React. While it can have a brain-bending learning curve for people new to functional programming concepts, we&#8217;ve found most people can become comfortable with the concepts within a week and start pumping out features without much slowdown.

Last month I did a talk at a dev Lunch &#8216;n&#8217; Learn series on React. By this point various team members had covered different aspects of React and I was getting into the meat of Redux. I tried to cover Redux generally, and some basic functional concepts, but I did get into showing some implementation as well as some cool packages to use with Redux. I recorded the presentation and, apart from an echo-chamber of a room and an interlude in the middle where people decided they needed a second-helping of our delicious lunch from [Sichuan Dynasty](http://www.sichuandynastyop.com), I thought it went pretty well. It was the first talk I&#8217;d ever done, and I have lots of stuff to improve, but I was happy with it for a first attempt.

As an aside: Our team was used to Gulp + Browserify as it had been used it on a previous project, and feature priorities kept us from implementing HMR like we&#8217;d wanted, so we hadn&#8217;t yet fully realized the benefits of the Redux dev flow. However, recently I&#8217;ve had a little free time to switch over to Webpack and enable HMR for our entire project, which has been a wonderful experience.

I&#8217;ve thrown the project I used in the talk up [on my GitHub](https://github.com/timothymclane/react-redux-lunchnlearn) and the slides are available at [Slides.com](https://slides.com/tmclane/whats-redux-anyway). Any comments, questions, or corrections are definitely welcome.

<div class="jetpack-video-wrapper">
  <span class="embed-youtube" style="text-align:center; display: block;"></span>
</div>

&nbsp;