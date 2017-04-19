---
layout: post
title: "Falsehoods Programmers Believe About Language"
date: 2017-02-07 22:17:11 -0600
comments: true
categories: 
---

As is now some kind of tradition akin to "X Considered Harmful" posts of old,
here I am, a software developer, writing a post trying to lay bare some of the
[wrong ideas many developers hold about a broad topic](https://github.com/jameslk/awesome-falsehoods).
In keeping with my degree and a personal area of interest, let's look at some of
the things (Anglophonic) developers often believe about language that do not
_quite_ track to reality.


## Translating my application into other languages isn't important.

It's true that many people in the world who speak more than one language choose
to learn English and that many of those people speak "better" English than many
native English speakers. (I put "better" in quotes because that idea is pretty
judgemental against certain dialects of English that often have racist and/or
classist components).

However, there's a big difference between being _able_ to use an application and
being _comfortable_ using an application. If two competing applications exist
and one is in a user's native language, they are very likely to pick that one
even if it doesn't have all the bells and whistles. Sadly, I don't really have a
good example for this, but I hear that WhatsApp vs Facebook Messanger might be a
good comparison.


## Translating a single word of English is simple.

Think of all the places in software where just a single word appears. If a
translator is only presented with a YAML file of strings, how are they supposed
to know whether it's "Back" as in "go back to where you were before" or as in
"not the front"? Whether it's "Post" as in "post my comment, now" or as in
"mail"? This can lead to some very confusing experiences for non-English users.

Also, if we consider just verbs, often tenses collide in English. "Eat" appears
in all of: "I will eat this" (future), "I eat this (often)" (habitual present),
"Did you eat this" (past) and "I was going to eat that" (conditional perfect, I
think. Who even names these things?!?).

If we consider just _nouns_, some lanuages have what's called a case system
which is used instead of word order and/or prepositions to denote a noun's
relationship to the rest of the words in a sentence. So "gift" could have a
different shape in translations of these sentences:

* The gift is ready.
* I gave the gift to you.
* I threw a ball at the gift.
* The gift's wrapping is pretty.
* This is my gift.

I don't speak any declining languages well enough to share an example (if you
do, let me know and I'll edit this post), but you can see this in effect in
English pronouns: who/whom/whose, I/me/my/mine, we/us/our/ours, he/him/his/his,
she/her/her/hers (notice the last two have the same number of forms, but collide
in different places).


## Short words in English are short in other languages.

If your GUI has a small button for "Post" your pixel perfect design will fall
apart in some other language that has a longer word for it (compare just the
English "Publish"). This isn't just a feature of languages with smallish
speaking communities. German is _notorious_ for forming new words by making
compounds and they can get quite long compared to their English conterparts.
E.g. "Nacktschnecke" (slug) or "Fledermaus" (bat).

There are a _few_ words that are short in almost all languages they appear in:
"the", "a", "mom", "dad", "I", "you", and things like that. These are words so
commonly used and so old that they've been whittled down over time for
efficiency's sake.  But most words that are short in one language are short out
of pure happenstance and some languages (English is one) seem to have a
preference for more shorter words while others prefer fewer longer words.


## Translating a whole sentence of English is simple.

A whole sentence as the smallest unit to be translated is better, but there is
still plenty of room for ambiguity. Consider any sentence with "it" in it.
In languages where every (or even some) noun is gendered, a translator needs to
know what the antecedent is or at least its gender in order to correcly
translate this word. If that antecedent is in a previous sentence, they'll have
to guess and if they're wrong, the sentence will read as if it were written by a
young child still learning the basics of grammar.


## Templating sentences adds only a little complexity.

It's common practice in the software industry to create templates out of
sentences and substitute in short phrases or single words. You might see
something like `"#{user.name} is in the #{user.location}."` where the user's
name is, well, their name like "Ben Hamill" and the location might be one of
"yard", "kitchen", "bathroom" or "bedroom". That seems reletively
straightforward... in English.

In languages with gendered nouns, those four location words might have different
genders, meaning that the untemplated "the" would be different for different
locations. If the translator lacks the context for what might go into that slot,
they'll have no hope of being right all the time. For instance, in Spanish, "the
kitchen" is "la cocina", but "the bathroom" is "el baño". If the translator has
to pick either "el" _or_ "la", they're guaranteed to be wrong some of the time.


## If I make sure to group things like articles in with nouns, that'll sort gendered languages.

If you change the above example to `"#{user.name} is in #{user.location}."` and
then make the location options "the yard", "the kitchen", "the bathroom" or "the
bedroom", you _will_ sort out that particular sentence (as far as I know, but
see below). But consider something like `"Your friend is wearing a #{garment.color} #{garment.type}."`
Many languages expect adjectives to agree on gender with the noun they modify.
If your friend could be wearing a shirt or a hat that can be red or purple, you
need two of each color and need to be aware of which garment needs which version
of the appropriate color.


## I can write enough grammatical metadata to be able to stitch together coherent sentences in all the languages I'm targeting.

There are whole research projects by leading linguists and computer scientists
working on this problem. Solving this in general is _ridiculously_ hard, it
seems. Even solving it for one specific pair of languages requires you to know a
ton about the grammatical rules of both languages and most native English
speakers cannot express
[the grammar rules they routinely adhere to on the fly as they speak](https://dictionary.cambridge.org/us/grammar/british-grammar/about-adjectives-and-adverbs/adjectives-order),
let alone with such rigor that a computer could make use of the instructions.
Also, the amount of metadata you'd have to have about everything that might
appear in a template and every template or every slot in every template is
pretty unwieldy. Unless this is your company's core competency, you should
probably be solving other problems with your time.


## Well, templating is bad, but if I allow for two versions of every English sentence, I'll be OK for gendered languages.

This is wrong in two ways. First, two and one are not the only options for
number of grammatical genders languages have. Some languages have masculine,
feminine and _neuter_ nouns like German, Icelandic, Russian, and Polish. And
some languages (with, I'll admit, reletively smaller speaking communities) have
even more like Czech, which has four. Wikipedia, of course, has a
[big, categorized list](https://en.wikipedia.org/wiki/List_of_languages_by_type_of_grammatical_genders).

Second, if you have more than a few spots of semantic variability in a given
sentence, you may run into a combinatorial explosion just going from English to
a language with two grammatical genders. Similar to the above example about
clothing color, consider `"Your friend sees your teacher."` This is sort of
contrived to keep the other grammar simple, but could be any sentence with two
titles for nouns in it.

Spanish wants to know if your friend is gammatically male (_amigo_) or female
(_amiga_) and then wants to know the same thing about your teacher (_profesor_
or _profesora_). So you go from having one English sentence ("Your friend sees
your teacher.") to four in Spanish:

* Tu amigo ve a tu profesor.
* Tu amiga ve a tu profesor.
* Tu amigo ve a tu profesora.
* Tu amiga ve a tu profesora.


## At least templating users' names in is safe.

It is my understanding that in Russian, which I do not speak or know much about,
a verb has to agree in gender with the subject of the sentence, _only if it is
past tense_. For those who haven't thought about a sentence diagram since the
7th grade or whatever: The word for the action being taken has to agree in
gender with the person taking the action, but only if the action is in the past.

So something like, `"#{user.name} liked your post."` becomes fraught because
"liked" is translated differently depending on the grammatical gender of the
person who liked your post.


## I can reword my copy to avoid these kinds of pitfalls.

I really only speak English. I can sort of stumble my way around being
understood in Spanish and I know a few random phrases in French and German and
assortment of other languages. I know the first line of the _Ozymandias_
pedestal in [Sindarin](https://en.wikipedia.org/wiki/Sindarin) becauase of
course I do. Learning a language is hard, especially if you're interested in
learning it well enough to know which sentence structures are going to be harder
or easier to map back and forth to English.

Even assuming you learned all 11 or 14 or howevermany languages your application
is targeting, imagine trying to find the safe points in that 11- or
14-dimensional grammatical graph. That sounds hard to me unless you want to give
over control of all copy to some sort of neural network. Even if you managed to
find the limited vocabulary and sentence structure sets that were safe and easy
to translate between all the languages you support, it would be highly
constraining in how you can express ideas to users. If you could pull that off,
I'm sure you'd end up with super stilted sounding prose in at least a few of
those languages.

The key lesson, here, is that natural human language is _highly_ variant,
espeically when it comes to grammar. If you are not exposed to languages outside
of English or only exposed to the most-commonly-taught-in-America Romance
languages, you should assume you have no idea which aspects of a language even
_can_ vary, let alone _how_ they will vary. For best results consult an expert
and provide them as much context as humanly possible.

_Thanks to Dave McLain, Kristján Pétursson and Ralph Tice for helping edit this
post._
