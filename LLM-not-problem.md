# Is negativity a problem for LLMs?

## Background:
Vscode is popular I/SDE (integrated software development environment) for javascript and especially Typescript. Angular is a popular HTML/Javascript development tool, that uses Typescript, and bundles all the application for sending to a browser. Vscode has facility to build the Angular app, open a Chrome window to display the app and use Chrome dubugger API to enable debugging in Vscode (with the source editor window integrated with the debugger breakpoints, stack trace, local values, etc)

## A common problem for javascript developers

A common problem, especially with deep stack of 3rd-party software tools -- especially when the source code is Typescript that is transpiled into javascript which may be further tranformed by minification -- is to keep the source code editor aligned with browser's view of the transformed code. There are thousands of questions and answers to the various problems that arise from the numerous ways that can go wrong, all documented and disussed on the web: reddit, StackOverflow, individual blogs...

## A problem for LLMs: negativity

If one prompts an LLM (say ChatGPT or Gemini) with a query that triggers the key words: javascript, angular, typescript, vscode and debugger breakpoint, the LLM will find plenty to say. It will happily tell you all the common mistakes one can make, and the ways to fix those. However, if you want to know why the Vscode debugger fails to source-align with the Chrome browser when using Angualar-18+ it utterly fails to provide any meaningful help. **Because**: it sees all the 'relevant' text from the web and repeats that as if it were a solution.

In this case, the LLM went into great detail of how to solve the problems that arose in the past 10 years configuring Angular and Webpack, suggesting that one would need the 'customWebpackConfig' extension, etc. Eventually I reminded the LLM that (A) the sourcemaps are working perfectly in the browser and (B) Angular 18+ uses Vite instead of Webpack. One would hope that a real intelligence would then stop offering how to configure Webpack to generate better sourcemaps; but no.

The problem for the user of the LLM is how to express and have **the LLM understand a *negative***. How to express that the problem is *not* such-and-such, and filter out all the accumulated text that addresses such-and-such. ('such-and-such' in this case: minification, URL mapping, generated sourcemaps, embedded sourcemaps, etc.)

One can tell the LLM: "Chrome debugger works, it has (unminified) source and working sourcemaps but vscode does not have the same source", that should narrow the responses and reduce the irrelevant noise, but it does not. 

Well, sure, the LLM will politely reiterate that Chrome is working and the sourcemaps must therefore be correct, but vsocde is not working so the problem must be in vscode... it's hard to keep the tools aligned... do you have the latest version... \[ironic, because the problem only manifests when using the latest versions\]. 

But that lasts for only one utterance, then back to the regurgitating the keyword-hit advice/solutions to un-minify, generate sourcemaps, etc.

### in the end...
After some pushing, the LLM would admit that Angular-18+ uses Vite instead of Webpack, so *there* is a clue. The LLM tries to help by suggesting how to find the detailed logs from vscode. But that just leads down a rathole of URL mapping, as if vscode could access the source through a URl to get the same code the browser was accessing. 

FTR: the root cause is actually that Vite transforms the code (inserting newlines) when sending it to the browser, but the transformed code is not available outside the channel between vite and Chrome. So vscode, using the original source in node_modules/.., is hopelessly lost. The only recourse is to configure Angular to revert to Webpack and forego Vite (lacking a way to tell Angualar to tell Vite to stop modifying the source)

Even after the solution has been identified the LLM barks about all the reasons why one should continue to use vite (basically: it is faster); even though the original and all following prompts were specifically about how make vscode debugger work with the browswer breakpoints (and vite fundamentally breaks that by using its own source). No surprise that an LLM cannot follow a chain of logic; but why do we all it "intelligence"?

The 'good' news: LLM confidently/helpfully presents what it found on the web, the 'bad' news: in cannot stop itself from repeating *all* that it found on the web. 
