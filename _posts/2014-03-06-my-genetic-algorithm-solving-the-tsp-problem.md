---
layout: post
title: My Genetic Algorithm Solving the TSP Problem
date: 2014-03-06 13:59:48.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- Algorithms
- CPlusPLus
- Genetic Algorithms
- Traveling Salesman Problem
- Visual Studio
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
  _oembed_5c36a0a12d5ad05ee3f74ec40e585280: "{{unknown}}"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p>I wrote a <a href="http://en.wikipedia.org/wiki/Genetic_algorithm" title="Genetic Algorithm">genetic algorithm</a> (GA) back in 2011 to solve the <a href="http://en.wikipedia.org/wiki/Traveling_salesman_problem" title="traveling salesman problem">traveling salesman problem</a> (TSP). I wrote the algorithm in C++ and made a recording of the algorithm that I later uploaded on YouTube. It was during the time that I was obsessed with algorithms. While having a major interest in GAs, my primary focus was on writing a new algorithm to efficiently solve the <a href="http://en.wikipedia.org/wiki/Boolean_satisfiability_problem" title="boolean satisfiability problem">boolean satisfiability problem</a> (SAT). Which I eventually did, getting my results <a href="http://www.scirp.org/journal/PaperInformation.aspx?paperID=23881#.Uxh5_vldV1A" title="Scientific Research">published in the Scientific Research Journal</a>. Oh, those were fun times! So the recording of the GA has been on YouTube for close to 3 years, and has over 10000 views today. I thought I'd share it with you:</p>
<p>http://www.youtube.com/watch?v=lgSxgiEAql4</p>
<p>The video shows my algorithm solving the TSP problem, where the number of cities is 100. I used an elitist approach here, with 60 % order crossover, 10 % mutation and the algorithm ran for 10000 generations. What you see is the cities being plotted on a circle, and the different paths being computed by the algorithm. Instead of showing you the different paths, I probably should have only showed you the best paths being picked (elitism). But if I had done that, you wouldn't have been able to see so many pretty lines being drawn.</p>
<p>If I were to re-write the algorithm today, I would have made it better in many ways. Primarily in terms of readability of the code, but also structurally. Oh, and I wouldn't use a crappy laptop with 2 GB of RAM :)</p>
