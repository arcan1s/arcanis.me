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

# Disclamer

As I said this guide does not explain _each_ skill, just does some core skills
and their basic usage, so please read skills descriptions first. Also it is not
the best guide, but it is just one of point-of-views by raiding healer. Have fun
and leave comments under paper.

**NOTE** this guide is currently under development.

# Stances

Astrologian has two stances:

* Diurnal Sect

  Your stance when you are going to play as HoT-based healer. Currently (4.01) it
  busts your healing potency by 10% and adds HoT effects to Aspected skills.

* Nocturnal Sect

  Your stance when you are going to play as shielding healer. Currently (4.01) it
  busts your healing potency by 15% and adds shielding effect to Aspected skills.

# Healing efficiency

* `HPS = (Potency * Targets) / max(cast, recast)`
* `Efficiency = HPS / MP`

## Main skills

| Skill | Potency | Base HPS (for all party members) | Efficiency | Comment |
|-------|---------|----------------------------------|------------|---------|
| Benefic | 400   | 160                              | 0.33       | Main healing skill if targets HP is above 50-60% |
| Benefic II | 650 | 260                             | 0.24       | Main skill if targets HP is below 60-70% |
| Helios | 300    | 960                              | 0.67       | Main AOE healing skill |

## oGCD skills

General rule - use them by CD.

| Skill | Potency | Comment |
|-------|---------|---------|
| Essential Dignity | from 400 | Main oGCD healing skill, as soon as it is scales on target's HP should be never used if HP is above 50% |
| Collective Unconscious | 150 * (15 / 3) = 750 | See notes below |
| Earthly Star | 720 | See notes below |
| Lady of Crowns | 500 | See notes below |

## Stance based skills

General rule - never spam any of it.

| Skill | Potency | Base HPS (for all party members) | Efficiency | Comment |
|-------|---------|----------------------------------|------------|---------|
| Aspected Benefic/Nocturnal | 200 (200 + 200 * 2.5 = 700) | 80 (280)   | 0.06 (0.19) | Should be used when you need to shield single target or when you need to heal someone as soon as possible |
| Aspected Benefic/Diurnal | 200 + 140 * (18 / 3) = 1040 | 416    | 0.29 | Should be used when you need to heal someone as soon as possible, you might wanna keep up it on tanks |
| Aspected Helios/Nocturnal | 150 (150 + 150 * 1.5 = 375) | 400 (1000) | 0.22 (0.56) | Should be only used to mitigate incoming party damage |
| Aspected Helios/Diurnal | 200 + 40 * (30 / 3) = 600 | 1600    | 0.89 | In general you wanna keep it up for party members after incoming AOE damage leaving about 80% on each party member, for example it was my main skill during first phase of A10S and A11S. Avoid use it if party's HP is about full and no incoming AOE damage expected in 10-15 sec |

Short notes:

* As far as you can see Diurnal sect is more powerfull for healing abilities than
Nocturnal (but I'm not taking into account difference in healing power).
* Benefic is more efficient than Benefic II, but requires additional GCD, which
can be used to pew-pew.
* Helios is not such good as Aspected Helios/Diurnal is (unlike WHM Cure III of
which is the best).
* Aspected Benefic/Diurnal is even better than Benefic II.

# Core mechanics

Actually astrologian not only healer (I think it is even _not_ healer), but it is
party buffer. Each buff can be improved by Royal Road. The following table
represents basic card usage in my opinion

| Effect | Arrow | Balance | Bole | Ewer | Spear | Spire |
|--------|-------|---------|------|------|-------|-------|
| None | Good choice in case if you don't have balance. Basically the best card for BLM, not bad for other casters (including healers). Please note that frequent usage on resource limited jobs (like physical DPS) may cause issues with these resources (TP for physical jobs, MP for casters). _Does not_ increase DoTs potency | Your main buff, any DPS likes it, but better use it to the best one, because it scales on current DPS value. To be honest in my practice it may cause some agro issues as well | Good for tanks especially when no CDs left | Can be used for SMN after ressurection or to healer, but better Royal Road it | Never use it, Royal Road or Arcana is your choice | Can be used for physical DPS after ressurection or on TP issues, but better Royal Road it |
| Enhanced | Bad combination, the only one good usage is BLM buff | The best choice for this improvement, should be always followed by Time Dilation if possible | Good for incoming tank buster, you might wanna Time Dilation it as well | Never use it | Never use it | Never use it |
| Expanded | A bit worse alternative to AOE Balance, use it if no other choices | The best buff from Astrologian. Should be always used on pull. Don't forget Celestial Opposition it if possible | Situational buff, I found it useful on savages tries in minimal item level to mitigate incoming large AOE damage, you might wanna extend MT buff as well, more likely should be never stacked with Celestial Opposition | The worst buff you can use | The worst buff you can use | The worst buff you can use |
| Extended | Same as non-improved version | Same as non-improved version | Same as non-improved version | Never use it | Never use it | Never use it |

## Minor Arcana

* Never hold it. Lady of Crowns is a good oGCD alternative to Benefic II.
* If you don't see usage for current card in 10-15 sec, and can't/don't want hold
it or Royal Road - convert it to Minor Arcana.

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

Should be used to mitigate incoming AOE damage basically by CD. The one of the
best usages is on rephases (if there is incoming damage of course), because it
can be extended by Celestial Opposition with damage buffs after (in this case be
sure that you have already drawn card and ready to buff party).

## Celestial Opposition

The one of questionable astrologian skills. In my opinion the only one usage for
it is to extend party Expanded buff (like Balance or Arrow). Never use it to
extend your own buffs (e.g. Lucid Dreaming). Also I should note that it can be
used to stun multiple enemies if it is required by fight mechanics (like A6S
adds).

## Earthly Star

In short - know fight. Teach your party to stay in circle (it is easy, trust me,
just don't heal outsiders).Despite the fact that you basically want to use it by
CD, it is better to hold it a bit to heal party during incoming AOE damage. The
best usage should be in about 15 sec before incoming damage - so you can detonate
sphere in any time and will get maximal healing and damaging effect.

## Sleeve Draw

* Should be used by CD.
* Should be never used if you have drawn card.
* Should be never used if you have Minor Arcana.

You _can_ use it if you have holded card or you already have additional effect.

# The best opener deck

* Hold Balance
* Royal Road Ewer or Spire
* Lord of Crowns
* Balance is drawn

Never use Sleeve Draw for your opener, let party wait for your deck, trust me,
they wanna have good numbers during opener. Expanded Balance should be followed
by Celestial Opposition.
