+++
title = "Quest for personal document management"
date = 2023-03-26
+++

Living in Germany you tend to collect a lot of paper. I mean physical,
dead-tree, paper. And a lot of it. Receipts! Invoices! Appointment
confirmation! Prescription! Many many important things.

To an untrained ear, this might not sound like a big problem. I used
to think the same way. And I never thought I'd face a problem like
this ever in my life. Even after moving to Germany, it never really
bothered me. That was until I had to scour through stacks scattered
around my apartment to find a certain document.

To combat this, I decided to digitize all my documents. So I bought a
scanner. It also came with a printer. That was no accident. Thing is,
here you need both. I'd been asking my friends to print my documents
until then.

I spent a weekend scanning almost all of my documents. I moved them
all into an external SSD. Named and grouped them as best as I could.
All my important documents, scanned, and labeled, right there on the
table. I felt a huge weight was off of my shoulder. Never again will I
search in despair.

Fast forward a year later, I'm facing the same problem. I have a
growing backlog of documents that I've yet to digitize. And searching
for documents seems to take longer than before.

What happened? Turns out I did a half-ass job of building it. There
were a few things that I didn't take into account.


## Friction {#friction}

Friction to add new documents resulted in a large backlog. I wanted to
scan the document and not use the mobile camera for that because
scanning on mobile is a hassle. Things started piling up.


## No full text search {#no-full-text-search}

Lack of full text search becomes very noticable whenever you need to
find a document. It's not a big deal if you're good at categorizing
your files and building a hierarchy. But I found myself scour through
directories to find the right one time and again.


## Physical counterparts {#physical-counterparts}

I also negelected to find a system to store the physical copies.
Having just a digital version is enough for certain types of documents
but there are some that I need to keep the paper version as well.
Currently they're scattered across different boxes in the apartment. I
need an efficient system to store and index those for easy retrieval.

This is not a new problem so I'm confident that I'll find a lot of
inspirations on the internet.


## Next {#next}

I've decided to give this another go. I'm also looking into leveraging
some tools. Currently I'm testing [paperless-ngx](https://github.com/paperless-ngx/paperless-ngx). It's a fantastic
piece of software. It's also built using Django which is a plus for me
because I have experience with it.

What I like about paperless-ngx is that it doesn't have a lot of
friction when it comes to adding new documents. It does a really good
OCR. It has a good full text search.

There are also other web based tools similar to this. I briefly
installed [papermerge](https://www.papermerge.com/) and gave it a
spin. The papermerge UI didn't click it for me.

I haven't stopped looking though. While paperless-ngx is very capable,
I'm trying to avoid having a server. I could run them on my laptop but
that's not ideal. A local-first solution would be great. [OpenPaperwork](https://openpaper.work/en/)
seems like something I want. It promises OCR and quick search. But it
doesn't seem to run on MacOS.

I'm also tempted to write a bare-bones solution myself. I could use
[tesseract](https://github.com/tesseract-ocr/tesseract) for OCR and build a full-text search index with SQLite. For
extra portability, maintain a metadata file next to the original file
that stores the extracted text, tags, and other things. But I'm
holding off for now and see if there's anything else.