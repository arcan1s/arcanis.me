---
category: en
type: paper
hastr: false
layout: paper
tags: ffxiv, astrologian
title: FFXIV Astrologian guide
short: ffxiv-astrologian
---
Astrologian is stance based healer, which can an fill in both main- and off-
healer roles. This guide aims mostly to help new players which just got job
crystals and have only vague idea how to play as an Astrologian. On the other
hand I hope some experienced players may find some useful tips as well. Please
note that I'm not going to describe each skill, because there is a very detailed
official Astrologian skills and abilities guide [here](http://na.finalfantasyxiv.com/jobguide/astrologian/)
which can be used for further reference.

<!--more-->

# Disclamer

As I said, the primary goal of this guide is to discuss general Astrologian
gameplay, core skills, their basic and advanced conjoint usage. This implies
basic knowledge of skills and abilities, so please read skills descriptions first.
Also, take into account that this guide is just one of point of views of a
raiding healer and it aims mostly to discuss how to play in end-game content.
This guide does not teach you how to play in casual content (like usual 4-ppl
dungeons), but you can certainly use all advises and raiding practices in
casual content. No pictures (yet?).

The guide is actual for 4.05.

Special thanks to Nan Talion (Odin) and Marisha White (Odin) for reading this
template and pointing out some shortcomings and vaguenesses.

**NOTE** this guide is currently under development.

# Changelog

* Jul 20 2017: initial version
* Jul 21 2017: add gear information
* Jul 29 2017: add Marisha's remarks

# Terms and concepts

Before we bury ourselves with advanced stuff let’s introduce some basic terms
and concepts we will be using further.

* DoT (damage over time) is a damage inflicted continuously over a period of time.
* HoT (healing over time), same as DoT, is a healing inflicted continuously over
a period of time; regeneration.
* HPS is healing per second, and it reflects how much raw healing is done on the
average. In this guide we are using the following formulae:

  * `HPS = (Potency * Targets) / max(cast, recast)`
  * `Efficiency = HPS / MP_cost`

  So, for example Helios (base cast and recast times are 2.5 sec) affects 8 players
  and have potency 300: `(300 * 8) / 2.5 = 960`.

* oGKD skills are skills that are instant and don’t trigger global skills
cooldown.
* Overhealing is healing a target when it’s HP is already full. You can’t evade
it altogether, but general consensus is that too much overhealing is bad. It
means that a healer uses too much mana and generates too much agro. Both can
potentially cause a wipe if a healer runs out of mana or steals agro from tanks.
* Shield is a buff that, if applied preventively, reduces the damage received by
a target by shield’s potency.
* Main-healer, off-healer - no, it does not depend on your current stance. Some
people [say](https://www.reddit.com/r/ffxiv/comments/4u9cwk/in_raid_content_can_a_whm_put_out_more_dps_than_a/d5o3ej0/?context=3)
that there are no main-healers and off-healers, but in general they are doing it
wrong. Off-healer means that they heals from time to time, while main-healer deals
damage from time to time, i.e. _any healer should deal damage_. Moverover current
content mostly allows statics to be almost solo healed even with damaging
"main-healer" (and yes, I'm talking about casual statics), especially if we are
talking about SCH-AST combination because of fairy.

# Stances

There are two stances which an Astrologian can use. These stances determine
additional effects of two skills, Aspected Benefic and Aspected Helios. That
might seem not too much at first glance, but stance effects of these two skills
are what determine two very different Astrologian play styles and her or his role
in raid.

* Diurnal Sect

  Your stance when you are going to play as HoT-based healer, boosts your healing
  potency by 10% and adds HoT effects to Aspected skills.

* Nocturnal Sect

  Your stance when you are going to play as shielding healer. boosts your healing
  potency by 15% and adds shielding effect to Aspected skills.

# Healing efficiency

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

For Benefic or Benefic II a window when you want to use it is 50-70%. The choice
of which skill to use depends on several factors. If you need to heal a target to
full HP as soon as possible, you will generally prefer Benefic II. But a tank
with 50% HP and a mage with 50% HP are two very different things because they
have very different HP pool. So using Benefic I for a mage might be more than enough.
It leads us to a very interesting thing: experimenting with our skills. Just try
to use Benefic II on as low HP as you (and tank) are comfortable. For example, I
found that during active movement and mechanics avoidance phases (also referred
as "dancing") on Susano Extreme Benefic I is more than enough despite the fact
that lightning damage is more than 50% of DPS' HP.

## oGCD skills

* Essential Dignity

  * potency: from 400 to 1000
  * comment: it is main oGCD healing skill. The main principle of use: the less
  HP the target has the better because it scales on target's HP; should be never
  used if HP is above 50%. Obviously it should be never used _after_ Benefics
  (exceptions are healing WAR/DRK after their Holmgang/Living Dead in which I
  prefer Benefic->Essential Dignity->Benefic II combination).

* Collective Unconscious

  * potency: 150 * (15 / 3) = 750
  * comment: see notes below

* Earthly Star

  * potency: 720 (fully charged)
  * comment: see notes below

* Lady of Crowns

  * potency: 500
  * comment: see notes below

General rule - use them as soon as they are up when appropriate.

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
  you might want to keep it up on tanks

* Aspected Helios/Nocturnal

  * potency: 150 (150 + 150 * 1.5 = 375 with shields)
  * base HPS (for all targets): 400 (1000)
  * efficiency: 0.22 (0.56)
  * comment: should be only used to mitigate incoming party damage

* Aspected Helios/Diurnal

  * potency: 200 + 40 * (30 / 3) = 600
  * base HPS (for all targets): 1600
  * efficiency: 0.89
  * comment: in general you want to keep it up for party members after incoming
  AoE damage leaving about 80% on each party member, for example it was my main
  skill during first phase of A10S and A11S. Avoid use it if party's HP is about
  full and no incoming AoE damage expected in 10-15 sec

General rule - never spam any of it.

## Short notes

* As far as you can see Diurnal sect is more powerfull for healing abilities than
Nocturnal (but I'm not taking into account difference in healing power though).
* Benefic is more efficient than Benefic II (by healing and MP usage), but
requires additional GCD, which can be used to ~pew-pew~ deal damage.
* Helios is not as good as Aspected Helios/Diurnal is (unlike WHM, whose Cure III
is the best).
* Aspected Benefic/Diurnal is even better than Benefic II.

# Core mechanics

Astrologian is not only a healer (I think it is even _not_ a healer), but a party
buffer. Astrologian core mechanics is using buff cards. It has 6 different buffs
which can be further amplified using another mechanics – Royal Road. Royal Road
has 3 variants:

  * enhanced – increases any card's effect by 150%.
  * expanded – reduces card's effect by 50% but applies it for all party members
  * extended – doubles card's duration

The following table represents basic card usage.

* Arrow

  * good choice in a case if you don't have the Balance. Basically the best card
  for BLM, not a bad for other casters (including healers). Please note that
  frequent usage on resource limited jobs (like physical DPS) may cause issues
  with these resources (TP for physical jobs, MP for casters). _Does not_
  increase DoTs potency. May has some other usages, for example, my co-healer
  found it is useful if you need to resurrect several people in limited amount of
  time with no Lightspeed and Swiftcast off cooldown (To be honest in my opinion
  it is trash uses anyway: Enhanced Arrow gives you 20% of 8 sec base cooldown
  which is still too low.)
  * enhanced: not the best combination, the best usage is to buff BLM
  * expanded: alternative to AoE Balance
  * extended: same as non-improved version

* Balance

  * your main buff, any DPS likes it, but better use it on the best one, because
  it scales on current DPS value. To be honest in my practice it may cause some
  agro issues as well
  * enhanced: the best choice for this improvement, should be always followed by
  Time Dilation if possible
  * expanded: the one of the best Astrologian buffs. Prefer to use it on pull.
  Should be extended by Celestial Opposition if it is possible
  * extended: same as non-improved version

* Bole

  * good for tanks, especially when no CDs are left
  * enhanced: good for incoming tank buster, you might want to Time Dilation it
  as well
  * expanded: situational buff, I find it useful during savages tries in minimal
  item level to mitigate incoming large AoE damage, you might want to extend MT
  buff as well, more likely should be never stacked with Celestial Opposition
  * extended: same as non-improved version

* Ewer

  * can be used for SMN after ressurection or on healer, but better Royal Road it
  * enhanced: never use it
  * expanded: never use it. This one is really never
  * extended: never use it

* Spear

  * any party member is good with this buff currently, but better use it on jobs
  which have crit-based traits - for example, BRD or MNK. Additionally, it is the
  only card that can increase healing power as it increases crit chance (it is
  especially noticeable for scholars or crit-stacking AST or WHM). Note that it
  still has less time than Balance or Arrow.
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
* In general Balance is still better than Arrow, because Arrow does not boost
DoTs and oGCD skills. On the other hand priority of Balance and Spear is disputed.

## Minor Arcana

* Never hold it. Lady of Crowns is a good oGCD alternative to Benefic II.
* If you don't see usage for current card in 10-15 sec, and can't or don't want
to hold it or Royal Road it - convert to Minor Arcana.

## Lightspeed

* Very useful when you need to heal and move at the same time.
* Very useful when you need to spam healing and want to safe some MP.
* Should be _never_ used when you are DPSing.

## Synastry

Situational skill which should be used when you need to heal several targets,
usually the best target is tanking player (during add in A11S, while dancing on
Susano EX and so on). Almost useless when you want to increase potency of healing
to single target.

## Time Dilation

Should be only used to prolong card buffs, but it would be cool if you will prolong
other buffs (like Aspected spells) as well. Better usage is to prolong Enhanced
card versions, but please note that prolonged buff will not be stacked with your
Draw CD anymore.

## Collective Unconscious

Should be used to mitigate incoming AoE damage basically by CD. The one of the
best usages is on rephases (if there is incoming damage of course), because it
can be extended by Celestial Opposition with damage buffs after (in this case be
sure that you have already drawn card and ready to buff party). Please note that
you need some time to buff being applied, so you need to stay for several seconds.

## Celestial Opposition

The one of questionable astrologian skills. In my opinion the only one usage for
it is to extend party Expanded buff (like Balance, Arrow and Spear). Never use
it to extend your own buffs (e.g. Lucid Dreaming); if you really want to prolong
your own buffs align it with applying party buffs. Also I should note that it can
be used to stun multiple enemies if it is required by fight mechanics (like A6S
adds).

## Earthly Star

In short it is the best healing ability an Astrologian has. It is instant with
minimal animation lock, it's 720 AoE healing potency and it also deals damage,
and it has only 60 seconds cooldown. But it’s usefulness requires one very
important thing: you need to know each fight well to predict the best places to
use it. Teach your party to stay in circle (it is easy, trust me, just don't heal
outsiders). Despite the fact that you basically want to use it by CD, it is
better to hold it a bit to heal party during incoming AoE damage. The best usage
should be in about 15 sec before incoming damage - so you can detonate sphere in
any time and will get maximal healing and damaging effect.

## Sleeve Draw

* Should be used by CD.
* Should be never used if you have Draw off cooldown.
* Should be never used if you have drawn card.
* Should be never used if you have Minor Arcana.

You _can_ use it if you have holded card or you already have additional effect.
There are situations during boss fights when all of your card slots are empty.
It might be useful to wait have Expanded Royal Road before using Sleeve Draw.
That way you are almost always guaranteed to have one of 3 DPS increase cards in
your Draw or Spread slots (sometimes both). So if your draw CD is close and you
have Sleeve Draw off CD it might be beneficial to wait for Draw and hope to get
Expanded Royal Road first.

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

* Break - the only one usage if you need to Heavy targets, for example, orbs in
A6S. Mostly useless for DPS.
* Protect - by the way, you can cast Protect and replace it by another skill.
Usually one healer in party is enough to have it.
* Esuna - has no application in savages and extremes, but sometimes may be useful
in casual content. You should have it if there is Doom debuff (like Omega V4).
Usually one healer in party is enough to have it.
* Rescue - just move knockbacked target to you. The one of usage examples is
Susano Extreme.

## Trash

* Surecast - despite the fact that some tactics mean that healer should have
this skill (hello, Omega V4 Savage) I would recommend to avoid using it.

# The best opener deck

* Hold Balance
* Royal Road Ewer or Spire
* Lord of Crowns
* Balance is drawn

Never use Sleeve Draw for your opener, let party wait for your deck, trust me,
they want to have good numbers during opener. Expanded Balance should be followed
by Celestial Opposition. Balance can be replaced by Arrow or Spear, but they are
not so good as Balance is.

# Gearing and melding

In general you might want to prioritize stats in the following order:

Mind >> Crit Hit >> Determination ~ Spell Speed > Piety

* Some people [say](http://dtguilds.enjin.com/davidsastguide) that until you get
Crit Hit more than 2700, Determination is better. I cannot verify or deny it
because have no ability to do precise metering. In general Crit Hit is better
than Determination at least because of Lightspeed procs.
* Another question is Determination vs Spell Speed. Before 4.0 usually Spell Speed
and Determination had close values of stat weights. I would recommend you to
prioritize Spell Speed over Determination as it allows you to be more mobile.
* You should get Piety as much as you are comfortable. But it should have the
lowest priority. Try to use some Ethers. Piety is also depends on your party,
their experience and their equipment.
* You _can_ meld Direct Hit (healers gear does not have it itself), but it does
not affect your healing potency, so I would prefer Crit Hit.
* During savage progression I would recommend you to consider melding of some
Vitality to your accessories as it will allow you to survive massive AoE easier.
But as Piety it is better be replaced as soon as you are comfortable.

# Potions and food

Get some good HQ Ether potions and Mind ones. Ether can be used to restore your
MP (it is probably not so actual with 4.0, but still useful sometimes, for example
right after ressurection). Mind potions is a good alternative to Largesse.

In other hand you can choose any food you like and comfortable. I would prefer
to up Crit Hit and Spell Speed.

# Healing and damaging

Being a healer you must maximize your DPS and minimize your HPS. It also should
be accompanied by 99% uptime (when you cast anything), it means that if there is
nothing else to heal, you need to find something useful and effective to do -
deal damage.

* Never heal to 100%. The only one possible exception is healing tanks before
tank buster (I'm not talking about doing mechanics here - like Doom, or Living
Dead). Good choice is Aspected Helios with HoT effect.
* The previous means that in general you don't need to keep tank's HP full. 50%
and no tank buster expected? Okay, no heal for you then. Good example is Susano
Extreme first phase - the only one skill I used to heal MT is Essential Dignity
and Aspected Benefic sometimes.
*  The question is how much overhealing is OK. First of all, we usually have
regeneration applied on our main tank as it is the most effective way to deal with
general boss auto attacks. Sometimes these tics heal more than necessary because
the tank took less damage. And we can do nothing if our regeneration tics crit.
The second part of overhealing comes from healing mass damage when only several
party members suffered damage. For example, due to some mechanics 4 random people
from your party received damage and your main tank (MT) is also receiving damage.
If we use any of our mass-healing skills (Helios, Aspected Helios, Earthly Star),
we will heal 5 people, but for 3 remaining this healing will result as overhealing.
In my opinion you are good if your overheal is below 20-30%.
* Know fight and your cast time, try to heal tank right after tank buster. It
means that you need to start casting Benefic (II) during tank buster cast, not
after. Same for AoE healing.
* Use Benefic for spamming. It is more efficient and has Benefic II proc.
* Check if you have Benefic II proc, critical healing with Benefic II is cool.
(but don't use it if you can heal just by Benefic).
* If you are main healier - your goal is to heal party, don't hope and don't
trust your co-healer, if someone died because of lack of healing - it is your
fault. On the other hand if someone got additional damage because of mechanics -
teach them to heal themselves.
* On the other hand if you are off healer - healing is not your issue. Ask when
your co-healer needs help, and cast some Helios. But shielding tank on tank buster
is usually mandatory in this case. If someone dies you are responsible for
ressurection.
* As I said - know the fight. You don't need to heal party right after they got
damage, usually you can wait several GCD (or use Aspected Helios/Diurnal). If
they don't do mechanics they will die in this case of course, but it is not your
issue ("dead DPS has zero DPS" ©). P.S. Second Wind + HQ Max-Potion are amazing.
* Combust II should be always up. Good time to use it - while you are moving -
you can't cast anything with castbar, whereas Combust II has insta-cast.
* If you have more than 1.5 sec free - cast Malefic III. You probably don't want
to cast it too much having Lightspeed.
* For adds in savage content (like A12S) you can use Gravity, it can be
accompanied by Swiftcast by the way.

# Heal and resurrection order

In short - it depends.

## Resurrection

Several rules first:

* Don't cast resurrection until you have less than 50% HP. This rule is valid
for LB3.
* Don't cast slow resurrection until whole party have about full HP.
* Don't cast slow resurrection until moving phases. This rule is valid for LB3.
* Usually if more than 3 people died (and at least 2 of them are DPS'es) and
you have LB3 ready - you can use it.

1. Resurrect main-tank as soon as possible. You might be next in aggro list, trust
me, you don't want to die.
2. Resurrect jobs which are required by mechanics (e.g. caster/range LB).
3. If several people died prioritize resurrection for DPS jobs which can resurrect
too (RDM, SMN, especially RDM).
4. If you need MP here and caster or range DPS can give it to you - resurrect them.
5. Resurrect other DPS'es, you might want to prioritize people who does more
damage.
6. If you can heal solo (you should be able at least to do it) - resurrect your
co-healer one of last, or don't resurrect at all. In my practice I had several
A10S kills, when my co-healer was died during all fight after tractor. Exception:
you might want to resurrect your healer for shields.
7. Resurrect off-tank last unless you need them for tank swap or add.

## Healing

1. Heal yourself. Really, if you will die - whole party will die.
2. Heal tank which receives damage at the moment, don't waste MP to heal another
tank as soon as possible.
3. Heal party member which can be required by mechanics (e.g. caster/range LB).
4. Heal people with debuffs (like vulnerability up or after death).
4. Heal DPS. Does not matter how bad gear they have, they should have same priority.
5. Heal co-healer.
