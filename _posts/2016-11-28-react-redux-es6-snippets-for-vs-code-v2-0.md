---
id: 44
title: 'React Redux ES6 Snippets for VS Code &#8211; v2.0'
date: 2016-11-28T19:55:48+00:00
author: Timothy McLane
layout: post
guid: http://www.timothy.codes/?p=44
permalink: /2016/11/28/react-redux-es6-snippets-for-vs-code-v2-0/
dsq_thread_id:
  - "5504544125"
categories:
  - Tooling
  - VS Code
---
I recently updated my VS Code extension to support some dynamic snippets. I had this idea floating around in my head for several weeks, but I hadn't had much extra time to take a stab at things. This is a brief account of that.

## The Problem

In the original version of my extension, I didn't enforce any linting rules. Given JavaScript's dynamic nature and loose formatting requirements, I opted to not put any unrequired semicolons in my React-Redux snippets. I would have preferred something more dynamic to support different preferences, but my experience has been that most people don't use semicolons unless they absolutely have to. However, about a month ago my VS Code extension got a review that complained about semicolon usage which got me thinking, and I experienced some issues using my snippets at work where we use much stricter linting rules. I got some free time over the long holiday weekend, so I decided to fix it.

## The Fix

Updating the extension to support dynamic snippets required a full rewrite. In my search to find a solution that would give me what I wanted, I ran across some Stack Overflow and GitHub posts about CompletionItemProviders and how they could serve snippets. This was exactly what I wanted. I wasn't quite sure how to implement it, so I read all of the VS Code [Extension docs](https://code.visualstudio.com/docs/extensions/overview) and did a lot of Googling in search of a good implementation.

## The Implementation

I eventually settled on a language server and client to be able to serve auto-complete snippets. I based my implementation on the reference Microsoft [VS Code Language Server Node example](https://github.com/Microsoft/vscode-languageserver-node-example). As I didn't have time to totally rewrite the example in ES6, which I use every day at work, and given my prior experience with TypeScript, I decided to stick with the default TypeScript implementation and extended the example to serve my purpose. TypeScript is really nice to work with, and the value of compile-time type-safety cannot be overestimated. ES6 has lots of good tooling around it and the new features are really great, but nothing gives an answer quite as quickly as a compiler's scream.

The firstchange I made to the example was modifying the language server capabilities to add a completion provider.

{% highlight typescript linenos %}
let workspaceRoot: string;
connection.onInitialize((params): InitializeResult => {
  workspaceRoot = params.rootPath;
  return {
    capabilities: {
      completionProvider: {
        resolveProvider: true
      }
    }
  }
});
{% endhighlight %}

Once adding this, all I needed to do was provide a handler on the server to provide the client with a list of completion items and a handler to return an item, if selected.

{% highlight typescript linenos %}
import snippets from './snippets';

connection.onCompletion((textDocumentPosition: TextDocumentPositionParams): CompletionItem[] => {
  return snippets.snippets.map(item => {
    return {
      label: item.label,
      kind: CompletionItemKind.Snippet,
      data: item.data
    }
  });
});

function getSnippet(snippet: Snippet): string {
  return snippet.body.join("\n");
}

connection.onCompletionResolve((item: CompletionItem): CompletionItem => {
  const es6Snippet = es6Snippets.filter(snippet => snippet.prefix == item.data);
  if (es6Snippet.length > 0) {
    item.insertText = getSnippet(es6Snippet[0]);
    item.documentation = es6Snippet[0].description
  }
  return item;
});
{% endhighlight %}
I concatenate strings with the newline character because I didn't want to have to change the snippets from the snippets format—which is an array of strings, each representing a line of text—to full strings with nested new lines. By keeping the same format, it's a lot easier to import snippets from my workspace or individuals who want to contribute without much change, and it's a lot easier to work with the array of strings than concatenated strings.

In order to support rudimentary linter options, I added a configuration section that I pull in with an event.

{% highlight typescript linenos %}
connection.onDidChangeConfiguration((change) => {
  let settings = change.settings;
  linterRules = settings.reactReduxSnippets.LinterRules;
  if (linterRules == "Loose") {
    es6Snippets = snippets.loose;
  }
  else if (linterRules == "Strict") {
    es6Snippets = snippets.strict;
  }    
});
{% endhighlight %}


These can be changed in the workspace or user space settings, and are limited to the two values listed here. Anything else will cause VS Code to red squiggly the option and complain, which is a really nice feature.

I would have preferred to use an enumeration here instead of strings, but I had trouble getting that working quickly enough, so I just use string comparison. That combined with VS Code validating the config for me makes it &#8220;good enough&#8221;. Working code trumps broken &#8220;better&#8221; code.

## What's Next

I hacked this together in an afternoon, so it's not necessarily the cleanest version of the extension—and it might not even be the best way to do this—but it's functional and I'm relatively happy with how it works.

I might improve the extension by supporting more varied formatting options, or maybe clean up the code and build process as it's not quite where I'd like it. However, I've got a lot of interests and there are only so many hours in the day, so sometimes you've just got to declare it good enough and push to origin.

GitHub: [vscode-react-redux-snippets](https://github.com/timothymclane/vscode-react-redux-snippets/)
  
VS Code Marketplace: [React Redux ES6 Snippets](https://marketplace.visualstudio.com/items?itemName=timothymclane.react-redux-es6-snippets)