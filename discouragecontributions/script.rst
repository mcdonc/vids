========================================================
How To Discourage Contributors to an Open Source Project
========================================================

I submitted my first pull request (to documentation) in a popular open source
project.  I am no stranger to writing documentation.  The PR is solid; it
corrects some vague documentation in the core of the system.  It renders, and
the info in it is correct.

A member of the community of this project, who I suppose considers himself a
leader of some kind (or at least represents himself as one) made it very hard
to contribute.

This "leader" will literally respond to every question in the support forum
that flies by across every of his waking hours, dozens of them, hundreds of
them.  His responses, while often helpful, are often not.  Often he'll say
things like "just read the docs", "what are you *actually* trying to do?"
without even considering answering the question literally.  Terse, vague,
often-questionably-correct, semi-insulting answers to well-thought-out
questions; to *every question*.  That kinda guy.  It's a power trip.  This is
*his domain* and he wants you to know it.  He is *good at this shit*. You
aren't.

Why?  It may be that he has mild autism, and I can empathize with that.  But,
for better or worse, whether he's autistic or not doesn't change the fact that
at the moment, I think he's just kinda a fucking asshole.

We encountered the inadequacies of the docs at the same time in a support
channel.  This was the gist of the conversation::

    me: Man, these related function docs suck.

    him: They make total sense to me.

    me: They don't specify what any of the functions return.

    him: It's obvious what they are.

    me: From reading the source code?

    him: No, they all return the same thing.

    me: It isn't specified what they return in any of them.

    him: <argues more for merit of existing docs, implying I am maybe just not smart enough to understand them>

    me: no mas, I give up arguing about it here, but I can submit a PR as a suggestion.

    him: <no response for an hour>

    him: <sends a link to PR that "fixes" the docs>

    me: <thinks the PR stinks>

    me: could i suggest some fixes to your changes in a PR to your repository?

    him: no, add it as comments to the existing PR

    me: <thinks this is a shitty idea, because docs don't render like that>

    me: <does it anyway>

    <eight hours later>

    me: did you see my comment to your PR?

    him: <no response for two days>

So bottom line, after we argued about the utility of *doing anything at all*
for 10 minutes, he finally relented to my criticisms by making a PR, ignoring
my offer to do so.  But his doc changes still stank.  And he isn't really the
boss of me.  So, after waiting two days for him to review my comment to his PR
without response, I submitted a different PR to the project that didn't stink,
and referenced his PR within mine.  They addressed the same issue, it was just
that mine didn't stink.

When I submitted my pull request, instead of reading it, this community member
rewarded me by adding the comment "this is bad etiquette" to my PR.  No
explanation as to what he meant, although I kinda figured what it might be cuz
he's that kinda guy.

When I messaged him privately, he complained that I was "taking over his work"
without attributing him.  Note that I used exactly 15 lines of an example from
his PR in mine.  My PR adds 278 lines of code.  The "work" he was worried about
me stealing was a minute fraction of my PR.  The rest of it is totally
original.  And, to be frank, the only reason I used those 15 lines was more out
of *pity* cuz the rest of it fucking stank.  It wasn't like I couldn't have
made something original to replace it.  He was complaining that I was stealing
the concept of fixing this bit of the docs, I think. He hadn't even looked at
my PR.  And I *did* attribute him by linking to his PR in mine.

I argued that I asked him two days ago to look at my comment to his PR without
response in the support channel.  He has been around in the channel for those
two days.  He *quite literally* responds to almost every question in this
support channel, except, apparently, mine, which was directed at him
specifically instead of just being a generic question from somebody unlucky
enough to ask one and receive his "help" as a reward.

He said he didn't have time: "people have other things to do, you know."

I responded with "lol" because it's obvious that this guy has nothing *but*
time.  I suppose that was mean and unconstructive.  And then, more
constructively, with "why didn't you respond to my request to review it with
something like 'yep ill get around to it and notify you when i do'"?

I suppose then he thought about it, found himself trapped in an unwinnable
situation, and did indeed halfheartedly, begrudgingly apologize.  I then still
had to *ask him* to remove the "bad etiquette" comment.  He did.  But he
apparently still hasn't read my PR, nor has he incorporated any of my changes
into his.  Maybe he will.  Who knows?  Currently his PR and mine are
*competing*, which is fucking ridiculous.

Project leaders that want to discourage contributions should take his example.
These can be boiled down as such.

Make your project a tyranny
===========================

Tyranny is defined as:

- an elaborate set of ever-changing unwritten rules

- "rules for thee, not for me"

"No pull requests against my repository."
-----------------------------------------

Unwritten rule.

"Commits signify attribution, and attribution is terribly important."
---------------------------------------------------------------------

Unwritten rule.  And a good leader does not seek the limelight, but
celebrates team members for the work they do.

Rule for thee and not for me.  He asked me not to submit a PR against his
branch that fixed the docs, blocking me from doing anything at all, for some
unspecified reason.  If I had been able to do that, he would have gotten
attribution as the committer in the changelog, which would have been fine by
me, even if the content was mostly mine, I coudn't care less about attribution.
He would have told me to piss off if I asked him to not submit a PR against my
branch that improved things, because it was a structural change that needed to
be vaidated by code.  And that's what I should have done.  But I was trying to
be respectful.

Turn every support conversation into an "X-Y" problem
=====================================================

"What are you *actually* trying to do?"

Just shut the fuck up and answer the question if it's one that can be answered
literally.  Nobody is here for your power trip.  Nobody is paying you to be
here, nobody asked you to be here, nobody is forcing you to answer all of these
questions.  People appreciate it, but if you're there *expressly* for the
glory, your mental health is going to suffer and you're going to make the
project worse, not better.

Challenge the behavior of no one associated with the project
============================================================

"Oh, that's just John.  He's like that."

It's unsustainable.  I'm sure everyone in this community who has interacted
with him has some suspicion that this guy is toxic, at least unless they are on
the same place in the spectrum as he is.  But programmers are nice people, and
avoid conflict.  At least I do.  But sometimes it can't be helped.  Folks who
are old hands in that project should discourage this behavior, especially when
wanting to attract new contributors.  They will bounce off hard.  I've been
doing this shit for 25 years, so I don't bounce, but anyone else would have.
Real leaders *mentor* people, not do this clown shit, and they discourage the
clown shit when other leaders exhibit it.

In my case, there is an official moderation committee that I could complain to,
and I might if the behavior repeats.  And I suspect the behavior will repeat,
because this guy is just that kinda guy.  But I'll give him the benefit of the
doubt.  I don't like conflict.  But I'm not a project leader, either.  If I
were, it could be that this complaint is recursive. :)
