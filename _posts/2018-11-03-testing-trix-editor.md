---
layout:     post
title:      Testing the Trix Editor with Capybara and MiniTest
date:       2018-11-03 11:21:29 -0700
summary:    Testing Basecamp's Trix Editor with the MiniTest framework.
categories: ruby rails minitest trix
---

_Originally post on the [EightyTwenty Blog](https://medium.com/eighty-twenty/testing-the-trix-editor-with-capybara-and-minitest-158f895ad15f)_

![Capybara](https://source.unsplash.com/HlnTtGV0Yfg/600)

Basecamp’s Trix Editor is really an amazing tool. With it you can create fully functional and easy to use editors for your users that look great.

At Dringo we have a Ruby on Rails stack and love to back our code up with tests. I found that using Capybara with the Trix Editor is a bit confusing at first, so here is some useful code to help.

## Setup

First, here are the fixtures we’re working with:

<script src="https://gist.github.com/frankolson/82f431492f72124def83f49e9a922867.js"></script>

And here we make sure to autoload support files in MiniTest:

<script src="https://gist.github.com/frankolson/1f3c6ca5744b28d09eef6d0e410d8e41.js"></script>

## Implementation

We’re going to write two helpers to aid us in creating our tests. The first is a setter that interacts with the actual Trix Editor. The second is a getter, that finds the hidden input field Trix uses to store its value.

<script src="https://gist.github.com/frankolson/eaa7734795642eda837abf345242d430.js"></script>

Finally, these tests demonstrate how we can use helper methods:

<script src="https://gist.github.com/frankolson/06f4bf2dbd511131de668c40420e1219.js"></script>

Hope this helps. Enjoy!
