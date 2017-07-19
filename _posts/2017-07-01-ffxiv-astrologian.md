---
category: en
type: paper
hastr: false
layout: paper
tags: ffxiv, astrologian
title: FFXIV Astrologian guide
short: ffxiv-astrologian
---
This is small paper describes how to play astrologian. Astrologian is stance
based healer, which can play-main healer role as well as off-healer one. This
guide aims to help mostly new users which just got job crystal and have no idea
how to play as astrologian. In other hand I hope some experienced users may find
useful some tips as well. Please note I'm not going to describe each skill, so
for descriptions and so on please follow [the link](http://na.finalfantasyxiv.com/jobguide/astrologian/).

<!--more-->

# Disclamer

As I said this guide does not explain _each_ skill, just does some core skills
and their basic usage, so please read skills descriptions first. Also it is not
the best guide, but it is just one of point-of-views by raiding healer. Have fun
and leave comments under paper. Guide is actual for 4.05.

**NOTE** this guide is currently under development.

# Stances

Astrologian has two stances:

* Diurnal Sect

  Your stance when you are going to play as HoT-based healer, busts your healing
  potency by 10% and adds HoT effects to Aspected skills.

* Nocturnal Sect

  Your stance when you are going to play as shielding healer. busts your healing
  potency by 15% and adds shielding effect to Aspected skills.

# Healing efficiency

* `HPS = (Potency * Targets) / max(cast, recast)`
* `Efficiency = HPS / MP`

## Main skills

* Benefic

  * potency: 400
  * base HPS (for all targets): 160
  * efficiency: 0.33
  * comment: main healing skill if targets HP is above 50-60%

* Benefic II

  * potency: 650
  * base HPS (for all targets): 260
  * efficiency: 0.24
  * comment: main skill if targets HP is below 60-70%

* Helios

  * potency: 300
  * base HPS (for all targets): 960
  * efficiency: 0.67
  * comment: main AoE healing skill

You can see than Benefic - Benefic II window is 50-70%, it depends on several
factors: for example, if you need to heal target as soon as possible you would
prefer Benefic II. Just try to use Benefic II on as low HP as you (and tank)
comfortable - for example, I found that during dancing on Susano Extreme Benefic
is more than enough despite the fact that lightning deals damage on about 50% of
DPS' HP.

## oGCD skills

* Essential Dignity

  * potency: from 400
  * comment: main oGCD healing skill; as soon as it is scales on target's HP
  should be never used if HP is above 50%. Obviously it should be never used
  _after_ Benefics

* Collective Unconscious

  * potency: 150 * (15 / 3) = 750
  * comment: see notes below

* Earthly Star

  * potency: 720 (fully charged)
  * comment: see notes below

* Lady of Crowns

  * potency: 50
  * comment: see notes below

General rule - use them by CD.

## Stance based skills

* Aspected Benefic/Nocturnal

  * potency: 200 (200 + 200 * 2.5 = 700 with shields)
  * base HPS (for all targets): 80 (280)
  * efficiency: 0.06 (0.19)
  * comment: should be used when you need to shield single target or when you
  need to heal someone as soon as possible

* Aspected Benefic/Diurnal

  * potency: 200 + 140 * (18 / 3) = 1040
  * base HPS (for all targets): 416
  * efficiency: 0.29
  * comment: should be used when you need to heal someone as soon as possible,
  you might wanna keep up it on tanks

* Aspected Helios/Nocturnal

  * potency: 150 (150 + 150 * 1.5 = 375 with shields)
  * base HPS (for all targets): 400 (1000)
  * efficiency: 0.22 (0.56)
  * comment: should be only used to mitigate incoming party damage

* Aspected Helios/Diurnal

  * potency: 200 + 40 * (30 / 3) = 600
  * base HPS (for all targets): 1600
  * efficiency: 0.89
  * comment: in general you wanna keep it up for party members after incoming
  AoE damage leaving about 80% on each party member, for example it was my main
  skill during first phase of A10S and A11S. Avoid use it if party's HP is about
  full and no incoming AoE damage expected in 10-15 sec

General rule - never spam any of it.

## Short notes

* As far as you can see Diurnal sect is more powerfull for healing abilities than
Nocturnal (but I'm not taking into account difference in healing power though).
* Benefic is more efficient than Benefic II, but requires additional GCD, which
can be used to pew-pew.
* Helios is not such good as Aspected Helios/Diurnal is (unlike WHM, Cure III of
which is the best).
* Aspected Benefic/Diurnal is even better than Benefic II.

# Core mechanics

Actually astrologian not only healer (I think it is even _not_ healer), but it is
party buffer. Each buff can be improved by Royal Road. The following table
represents basic card usage in my opinion

* Arrow

  * good choice in case if you don't have balance. Basically the best card
  for BLM, not bad for other casters (including healers). Please note that
  frequent usage on resource limited jobs (like physical DPS) may cause issues
  with these resources (TP for physical jobs, MP for casters). _Does not_
  increase DoTs potency. May has some other usages, for example, my co-healer
  found it is useful if you need to ressurect several people in limited time with
  no Lightspeed and Swiftcast cooldown
  * enhanced: not the best combination, in my opinion the best usage is to buff
  BLM
  * expanded: alternative to AoE Balance
  * extended: same as non-improved version

* Balance

  * your main buff, any DPS likes it, but better use it to the best one, because
  it scales on current DPS value. To be honest in my practice it may cause some
  agro issues as well
  * enhanced: the best choice for this improvement, should be always followed by
  Time Dilation if possible
  * expanded: the one of the best Astrologian buffs. Prefer to use it on pull.
  Should be extended by Celestial Opposition it if possible
  * extended: same as non-improved version

* Bole

  * good for tanks especially when no CDs left
  * enhanced: good for incoming tank buster, you might wanna Time Dilation it as
  well
  * expanded: situational buff, I found it useful on savages tries in minimal
  item level to mitigate incoming large AoE damage, you might wanna extend MT
  buff as well, more likely should be never stacked with Celestial Opposition
  * extended: same as non-improved version

* Ewer

  * can be used for SMN after ressurection or to healer, but better Royal Road it
  * enhanced: never use it
  * expanded: never use it. This one is really never
  * extended: never use it

* Spear

  * any DPS is OK with this buff currently, but better use it to jobs which have
  crit-based traits - for example, BRD or MNK
  * enhanced: mostly jobs with crit-based traits, not such good choice for Time
  Dilation as Enhanced Balance is.
  * expanded: alternative to AoE Balance
  * extended: same as non-improved version

* Spire

  * can be used for physical DPS after ressurection or on TP issues, but better
  Royal Road it
  * enhanced: never use it
  * expanded: never use it. This one is really never
  * extended: never use it

Short notes:

* None of these rules (expect for Expanded Ewer/Spire) are mandatory.
* Never use macros for cards.
* Be sure that card is in CD or you have drawn a card and it is more than 15-20
sec left. If you have drawn one and you see no usages in more than 15 sec - try
to convert or hold it (or withdrawal it if no choices).
* Additional note to previous one: never let drawn card fade out.

## Minor Arcana

* Never hold it. Lady of Crowns is a good oGCD alternative to Benefic II.
* If you don't see usage for current card in 10-15 sec, and can't or don't want
to hold it or Royal Road - convert it to Minor Arcana.

## Lightspeed

* Very useful when you need to heal and move in one time.
* Very useful when you need to spam healing and want to safe some MP.
* Should be _never_ used when you are DPSing.

## Synastry

Situational skill which should be used when you need to heal several targets,
usually the best target is tanking player (during add in A11S, while dancing on
Susano EX and so on). Almost useless when you want to increase potency of healing
to single target.

## Collective Unconscious

Should be used to mitigate incoming AoE damage basically by CD. The one of the
best usages is on rephases (if there is incoming damage of course), because it
can be extended by Celestial Opposition with damage buffs after (in this case be
sure that you have already drawn card and ready to buff party). Please note that
you need some time to buff being applied, so you need to stay for several seconds.

## Celestial Opposition

The one of questionable astrologian skills. In my opinion the only one usage for
it is to extend party Expanded buff (like Balance or Arrow). Never use it to
extend your own buffs (e.g. Lucid Dreaming). Also I should note that it can be
used to stun multiple enemies if it is required by fight mechanics (like A6S
adds).

## Earthly Star

In short - know fight. Teach your party to stay in circle (it is easy, trust me,
just don't heal outsiders). Despite the fact that you basically want to use it by
CD, it is better to hold it a bit to heal party during incoming AoE damage. The
best usage should be in about 15 sec before incoming damage - so you can detonate
sphere in any time and will get maximal healing and damaging effect.

## Sleeve Draw

* Should be used by CD.
* Should be never used if you have drawn card.
* Should be never used if you have Minor Arcana.

You _can_ use it if you have holded card or you already have additional effect.

# Role skills

## Mandatory

* Lucid Dreaming - should be used to restore MP and/or to reduce generated agro.
Basically you want to use it by CD.
* Swiftcast - must be used for target ressurection. In general it is used to cast
skills with cast time more than GCD (Aspected Helios for example). In theory it
can be used to cast damaging skills while moving.
* Largesse - healing CD.

## Optional

* Cleric Stance - damaging CD, should be used by CD mostly, but should be never
used if you are going to spam heal.
* Eye for an Eye - actually I never used it, but someone may find it useful.

## Situational

* Protect - by the way, you can cast Protect and replace it by another skill.
Usually one healer in party is enough to have it.
* Esuna - has no application in savages and extremes, but sometimes may be useful
in casual content. You should have it if there is Doom debuff (like Omega V4).
Usually one healer in party is enough to have it.
* Rescue - just move knockbacked target to you. The one of usage examples is
Susano Extreme.

## Trash

* Break
* Surecast

# The best opener deck

* Hold Balance
* Royal Road Ewer or Spire
* Lord of Crowns
* Balance is drawn

Never use Sleeve Draw for your opener, let party wait for your deck, trust me,
they wanna have good numbers during opener. Expanded Balance should be followed
by Celestial Opposition. Balance can be replaced by Arrow or Spear, but they are
not so best as Balance is.

# Potions and food

Get some good HQ Ether potions and Mind ones. Ether can be used to restore your
MP (it is probably not so actual with 4.0, but still useful sometimes, for example
right after ressurection). Mind potions is a good alternative to Largesse.

In other hand you can choose any food you like and comfortable. I would prefer
to up crit and spell speed.

# Healing and damaging

Being a healer you must maximize your DPS and minimize your HPS. It also should
be accompanied by 99% uptime (when you cast anything). General tips are:

* Combust II should be always up. Good time to use it - while you are moving -
you can't cast anything with castbar, whereas Combust II is insta-cast.
* Never heal to 100%. The only one possible exception is healing tanks before
tank buster (I'm not talking about doing mechanics here - like Doom, or Living
Dead). Good choice is Aspected Helios with HoT effect. You are good if your
overheal is below 20-30% (some overheal is allowed because of HoT effects or if
you are going to shield members with full HP).
* Know fight and your cast time, try to heal tank right after tank buster. It
means that you need to start casting Benefic (II) during tank buster cast, not
after. Same for AoE healing.
* Use Benefic for spamming. It is more efficient and has Benefic II proc.
* If you are main healier - your goal is to heal party, don't hope and don't
trust your co-healer, if someone died because of lack of healing - it is your
fault. In other hand if someone got additional damage because of mechanics -
teach them to heal themselves.
* In other hand if you are off healer - healing is not your issue. Ask when your
co-healer needs help, and cast some Helios. But shielding tank on tank buster is
usually mandatory in this case. If someone dies you are responsible for ressurection.
* If you have more than 1.5 sec free - cast Malefic II. You probably don't wanna
cast it too much having Lightspeed.
* For adds in savage content (like A12S) you can use Gravity, it can be
accompanied by Swiftcast by the way.
