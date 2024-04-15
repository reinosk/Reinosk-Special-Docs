---
title: Welcome Aritlh's Wiki Site
---

This is Aritlh's personal Wiki site, mainly recording some scattered knowledge points summarized from his own words.

I've always felt that **knowledge should not be fragmented, but structured**. So I hope to find an elegant way to manage my knowledge.

I have learned about or tried methods such as OneNote, Evernote, Blogs, Wikis, etc. and summarized a set of processes for knowledge acquisition, absorption, and management that I think is relatively reasonable:

1. Use web pages, RSS, WeChat, and other online channels as sources of knowledge acquisition.

2. Use a note-taking system (handwritten notes, OneNote, Evernote) to initially filter valuable and targeted information, **bookmark** information, **classify and organize** it, and take notes on the **key points** after reading.

3. For the occasional one or two sentences, or very scattered ideas, inspirations, and initially digested content that come to mind, record them in a **Wiki**.

4. Finally, when there is a certain amount of content in a section of the Wiki, re-read the notes and content in that section of the Wiki, refine and distill them, add your own thoughts and understanding, and write an article to be published on a **blog**.

In practice, the **note-taking system** is mainly for **classifying and organizing** large chunks of information, the **Wiki** is mainly for **accumulating** scattered knowledge, and the **blog** is only for the **essence**.

When a section of the Wiki is first created, there may be very little content or just a sentence. But through this process, as you encounter more and more about that topic, the scattered knowledge also increases, and the content recorded in that section of the Wiki will also increase, which is to **structure and organize what was originally a large amount of fragmented knowledge through accumulation**.

The purpose of the blog is to **share and showcase** your knowledge and demonstrate your level of expertise. It needs to present something substantial, so it is not suitable for scattered snippets of knowledge. It is more suitable for presenting a **series** of summaries or tutorials, as a highly systematic knowledge platform.

When I tried to deploy my own Wiki system, I started to struggle with choosing which Wiki system to use. Referring to online blogs, I actually deployed and compared several popular Wiki systems: MediaWiki, DokuWiki, MDwiki, TiddlyWiki, wiz, vimwiki, Simiki, Wikitten, etc. None of them were satisfactory. Some were difficult and complex to deploy, some had ugly interfaces, some couldn't have nested categories, some nested categories couldn't be expanded, some couldn't search, and some even didn't use Markdown...

Based on these unsatisfactory user experiences, I summarized some of my requirements for a **personal Wiki**:

- Simple and beautiful interface with a friendly font and layout
- Support for multi-level categorization
- Easy to modify and update content
- Simple deployment
- Category directories can be expanded and collapsed
- When a category is expanded, you can view the titles of all articles/entries under that category
- Each article/entry can be assigned multiple categories/tags
- The Wiki should support internal linking
- Use Markdown to write articles/entries
- Support full-text search (searchable content and titles)

In my previous usage, Wikitten was the closest to my needs, but there were still some conditions that were not met, such as deployment and search. But personally, I liked the style of Wikitten.

So in the end, I chose to mimic the style of Wikitten and write this Hexo-based Wiki theme hexo-theme-Wikitten as my own Wiki system, which basically implements the above requirements. Now I'm using it for myself for the time being, of course, this initial functionality is still very simple, and there is a bunch of bugs and TODO lists that need to be maintained. I welcome anyone interested to submit PRs.