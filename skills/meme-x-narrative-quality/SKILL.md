---
name: Meme X narrative quality
description: >-
  Use when scoring a meme coin's narrative on X (100-point, ex-ante
  narrative-only, judged as of launch/re-ignition time), refining that rubric
  after new labeled examples, or checking why a coin just pumped in the last few
  hours.
---
# Meme X narrative quality (narrative-only, 100-point)

**Rubric version: v1.0.** Write `scoring_version` as `"v1.0"` on every report. Six-dimension weights are unchanged. Clarifications below were approved by the user on 2026-11-18 and stay inside v1.0 (do not bump to v1.1).

**Mode: narrative-only.** Judge the story itself as it looked at launch (or at the re-ignition moment for an old coin). Use only evidence timestamped at or before that moment. Never infer narrative quality from outcomes: price, post-launch heat, death, bundles, rugs. A survival/structural score is PAUSED until the user explicitly asks to train it; do not output one. Exception approved 2026-11-17: after all caps, apply the Dev risk adjustment below; it is a penalty on the already-capped total and must not rewrite any narrative sub-score.
Never invent numbers; tool-backed only, null if not found. No buy/sell advice, targets, or positions.

### Changelog
- **2026-11-23** (user approved 「确定」, 09:45 Asia/Bangkok / `2026-11-23T09:45:00+07:00`, stays v1.0; process only, no weight or cap change, historical finals unchanged): (1) `names_coin` evidence rule; (2) mandatory DexScreener same-name check every report; (3) `as_of` format and `base_score` definition fixed. See "Mandatory self-checks" in Output.
- **2026-11-19** (user approved 15:50 Asia/Bangkok / `2026-11-19T15:50:00+07:00`, stays v1.0): (A) research step 2a — check the token's own metadata links (DexScreener / pump.fun socials, website, X link) first when looking for the origin; (B) cap 1 clarified — an official IP / product / game name (e.g. a GTA VI full title appearing in Rockstar material) does NOT count as the origin naming the coin; cap 1 and cap 6 apply unless a pre-launch post literally names the ticker/coin. Historical finals unchanged.
- **2026-11-18** (user approved 「全批」, 19:49 Asia/Bangkok / `2026-11-18T19:49:00+07:00`): (1) event three-way classification; (2) mandatory snipe fields, ±10 minute window from the first verifiable public post, one-off events only; (3) Dev partial data is `数据不全` with no penalty, 中 only when the fields needed to decide are complete; ≥55 tier uses risk rules 2–4 only (restated). Historical finals unchanged (Fiona stays 34). `scoring_version` remains v1.0. JSONL layout is schema v2 (see `/workspace/meme_scout/proposals/schema_v2.md`); that is logging format, not a score change.
- **2026-11-17** (user approved): hard-date catalyst check; name wordplay; same-name scan across all launchpads; platform tokens scored as memes; hot big-news comment (no score change); Dev risk adjustment (tiered).

## Inputs
- Contract / ticker / chain; launch time (and re-ignition time if old coin)
- Optional user label `金狗` / `垃圾盘` (training label; still score independently, then compare)

## Research
1. Identify name/ticker/launch time.
2a. **Token metadata links first (approved 2026-11-19):** before searching X, open the token's own links (DexScreener info / pump.fun socials: X post or account, website, Telegram) and check each as an origin candidate. The deployer fills these in, so verify the linked post's timestamp is at or before launch (a post after launch can never be the origin) and judge it like any other source; a link alone earns nothing.
2. Find the origin: the post(s) the name/meme came from (author, followers, UTC+7 time, likes/views, URL), and the broader CT conversation or external event it rode. Check whether the origin post literally contains the coin name or meme words.
3. Check whether the exact meme/phrase was trending in the 24h before launch, and whether big accounts were riding the meme (not the CA).
4. Check name uniqueness: clones, homoglyph/Cyrillic ticker spoofs, same-name launches nearby (note which chain each is on and count same-chain same-name launches on the same day), celebrity name/initials used as ticker.
5. **Event classification and hard-date catalyst check** (classification approved 2026-11-18; date verification added 2026-11-17): first assign the narrative to exactly one of the three types in **Event classification** below, based on what the ticker and one-line story attach to, not on how hot that day's background is. If the class may be 有明确日期的外部催化, verify the official date with WebSearch/WebFetch (an official or major-outlet source published before launch; cite the URL). Timing may go up to 15/15 only when that class applies, the date is confirmed, and it falls within 72h after launch. Renewal for that class follows the Event classification section (not auto-capped at 6, not auto-filled to 20). Unverified dates are written as unconfirmed and get no timing boost. A one-off that happens on a release day is still 一次性突发事件.
6. **Name wordplay check (added 2026-11-17):** check whether the ticker/name is a pun, double meaning or in-joke (e.g. BAWSAQ = ballsack / Nasdaq parody). A recognizable built-in joke adds +1–2 to clarity and +1–2 to stickiness (within their caps); note it in the 加分 bullets.
7. **Same-name scan across all launchpads (added 2026-11-17):** the uniqueness check in step 4 must cover every launchpad and DEX on the same chain (pump.fun, bonk/letsbonk, Raydium LaunchLab, Meteora, Moonshot, custom vanity mints, etc.), not only pump.fun. Search DexScreener by name and ticker, and also check X for project sites with their own CA. List every earlier same-chain same-name CA found.
8. **Dev check (pump.fun only):** if this is a pump.fun launch (Solana mint on pump.fun, even if the address does not end in `pump`), run `/home/box/pumpdata/devcheck.py <full CA>` and map the JSON into the `dev` fields below. If the script fails or returns `{"error":...}`, treat as no data (`dev.risk` and `dev_penalty` are null). Skip non-pump.fun launches (BSC and other chains or launchpads): do not run the script; set `dev.risk` and `dev_penalty` to null.
9. Post-launch facts may be listed in one note line, but do not score them.

### EVM / BSC coins
- BSC launchpads include Flap (flap.sh) and four.meme; vanity addresses often end in 7777, 4444 or ffff. Take launch time from the launchpad's creation time or DexScreener's pair-creation time.
- Record the chain, the launchpad, and the pair asset. If the pair asset is part of the story (a tokenized stock like NVDAB, or a meme's source coin), mention it in the narrative.
- Chinese daily 金狗 / hot-list accounts are post-launch heat only, never the origin.

## Score
| Sub-dimension | Weight | High (≈full) | Mid | Low (≈0) |
|---|---|---|---|---|
| Source strength | 20 | Origin is a big/celebrity/official account or a post with ≥100K views that literally names the coin/meme | Mid KOL (10K–100K) or viral thread on the theme without the exact name | Tiny account (<1K followers), <5K views, deployer-type self-made |
| Meme clarity | 15 | One-second read for outsiders; image/character + name (e.g. DOGE+PEPE=DOPE) | Readable words but needs CT context | Unclear / inside reference |
| Renewal potential | 20 | External calendar/IP or recurring event keeps re-feeding it; catchphrase owned by the coin | Recurring CT phrase that can resurface but isn't ownable | One-off event, no second hook |
| Cultural stickiness / ritual | 15 | Character, pfp, chant, or ritual people perform (pfp up, "ni hao") | Some remix potential | Bare phrase, no image or ritual |
| Timing / attention fit | 15 | Exact meme spiking in the 24h before launch and big accounts riding it | Theme hot but phrase flat | No trend |
| Uniqueness | 15 | Original name/meme, first mover, no spoof | Generic name but first | Clone, homoglyph spoof, many same-name launches within an hour |

### Event classification (approved 2026-11-18)
Classify the narrative event into exactly one type **before** applying cap 2 or the 72h timing boost. The class is whatever the ticker and one-line story attach to, not how hot the day's background is.

| 类型 | Typical cases | How to score |
|---|---|---|
| 持续性叙事 | Lasting IP, a character, a reusable cultural symbol that does not depend on one date | Score all six dimensions normally. Cap 2 does not apply. No 72h timing boost. |
| 有明确日期的外部催化 | A scheduled game/film/match/election/earnings date that was published and verifiable before launch; the ticker or story **is** that calendar or that IP | Cap 2 does not apply. If the date is verified and falls within 72h after launch, timing may go up to 15. Renewal is scored as below. |
| 一次性突发事件 | Death, retirement, a single announcement, a celebrity one-liner, a one-off spoiler, a single breaking-news item, a one-time joke vote | Cap 2 is mandatory: renewal ≤ 6 and timing ≤ 10. Then run the snipe check; if it fires, subtract 3 from the total. |

Boundary when a one-off thing happens on a hard-date event:

1. If the ticker/story attaches to that one-off thing (a character name leaked on release day, a spoiler, a breaking news item that day) → **一次性突发事件**, cap 2. Do not raise timing to 15 or lift the renewal cap just because a release day is in the background. Example: Tomás `5ChyeKeH1Hv47qYd13E8ujg8n5Hn3YYPZtLS9DAUpump` (GTA VI release-day spoiler) is one-off.
2. If the ticker/story attaches to the scheduled IP or the release itself (the game's name, in-game currency, in-game exchange, official short name) → **有明确日期的外部催化**, even if spoilers or trending topics are also circulating that day. Examples: GTA$ `AQKJc3kbEcp7GBZpf5oTrmgxctYgk1Y77juGYHQvdbnA`, GTA6 `GTAeP5mqENEa9hJPQyutiirKKZxpHvKcpHZJ6ot5LEEk`, GTAVI `ERiM8NiCanarGEnWMnURAoxTuTvUXZbtJsPTirnHpump`, BAWSAQ (e.g. `D9EcoQZeAytCrF1kQFSpRNpoPhibJUvJcmRHXhubpump`), VI `3yjHj9s9MukEHr7b7qhMiUfH5fSUz97Eb5J8SRjdpump`.
3. Something that only looks like a date but is not an official published schedule (for example a rumor that the world goes dark on a given day) → still 一次性突发事件, not a hard-date catalyst.

Renewal for 有明确日期的外部催化 (this is what “renewal reflects the event window” means):

- Do not auto-cap it at 6, and do not auto-fill it to 20.
- If the dated event sits on an IP that will keep producing official material after the date (a game or film with post-release promo, footage, sequels), renewal may be mid to high. That only lifts cap 2's ceiling; the score must still follow the evidence.
- If the dated event is itself a one-shot calendar item (a match, an earnings print) — it has a date, but no ownable IP afterwards — renewal is mid-low, but it is not forced to ≤ 6. Timing may still go to 15 under the 72h rule.
- If the date was not verified before launch, or it falls more than 72h after launch: do not use the 72h rule to raise timing to 15; score timing by the ordinary “24h pre-launch heat” standard.

### Caps and floors (apply after base scoring; check every one and list each applied cap in `caps_applied`)
1. **Source must name it**: if the big origin post does not literally contain the coin name or meme words (the link is a community mapping/inference), source strength ≤ 12. **Official IP names do not count as naming (user decision 2026-11-19):** a game/film/brand/product title or official short name appearing in official material (e.g. "Grand Theft Auto VI" on Rockstar's site) is not a post naming the coin; if no pre-launch post literally names the ticker/coin, cap 1 applies (and cap 6 when it is also a later same-chain same-name coin). This applies equally to all IP-name coins (e.g. GTAVI, Rocco-type character names).
2. **One-off event cap** (mandatory whenever the narrative is classified as 一次性突发事件; do not skip it; does **not** apply to 持续性叙事 or 有明确日期的外部催化): single events (retirement, farewell, death, one-time announcement, one-off celebrity quip, one-off spoiler, one-off breaking news, a one-time joke vote) → renewal ≤ 6 and timing ≤ 10. Then run the **mandatory snipe check** below; if it fires, subtract 3 more from the total (after every cap and floor, before the Dev penalty).
3. **Uniqueness caps**: celebrity real name/initials or other generic ticker ≤ 2; later same-name coin (not first mover) on the SAME chain ≤ 4. If the same name launched first only on another chain and this coin is the first on its own chain, uniqueness ≤ 8 instead. If 3 or more other same-name coins launched on the same chain the same day before this one, uniqueness = 0.
4. **Grassroots floor**: if meme clarity + cultural stickiness ≥ 26, source strength ≥ 8 even when the first poster is tiny.
5. **Platform tokens** (launchpads/protocols with revenue, buybacks, airdrops, fee share; changed 2026-11-17 by user): score the narrative exactly like any meme (same six sub-scores, caps and dev adjustment). The mechanism itself earns no points; describe it separately in a `mechanism` JSON field and one line in the copy (e.g. 机制：手续费分红 95% + 回购销毁 5%). Tag `platform_token`.
6. **Unnamed-source + same-chain clone stack**: if cap 1 applies (origin does not name the coin) AND the coin is a later same-name coin on the same chain, total ≤ 40 (D).

6b. **Hot big-news comment (no score change; user rule 2026-11-17)**: when the origin is a big, widely covered breaking news event or one-off hot topic (cap 2 applies and the event itself is large, e.g. a major acquisition, a big announcement, a viral world news story), add a comment line in the copy right under 一句话: 点评：热点大叙事（打一波就跑，注意价格位置） and set JSON `comment` to "热点大叙事". It is a narrative-type label only: it never changes any sub-score, cap, penalty or grade. Otherwise `comment` is null.

7. **Strict enforcement of 1–6 (new launches only)**: a viral one-off project/post (e.g. a dev's demo going viral) always gets cap 2; an earlier same-name coin on the same chain counts for caps 3 and 6 even if it launched weeks earlier; a ticker that is a generic short word or abbreviation (VI, MMM, CRIME) gets uniqueness ≤ 2; a post timestamped after launch can never be the origin.
   **Re-ignition exemption** (苏醒 signals / 苏醒接力策略, i.e. an old, once-hot coin dying and coming back): do NOT apply rule 7. Judge at the re-ignition moment; the coin is the original project, so weeks-old same-name coins and posts between its first launch and the re-ignition do not make it a "later clone" or reduce source.

### Mandatory snipe check (approved 2026-11-18)
Every complete narrative report must fill the fields below. Fill them even when they do not apply; do not skip the block.

| Field | Meaning |
|---|---|
| `event_time` | Time of the **first verifiable public post** of the underlying event (official / news / celebrity remark / spoiler), with a numeric timezone offset such as `+07:00`. Not the coin-naming post, and not a later amplification or reprint. `null` if no verifiable event exists. |
| `launch_time` | Launch time, with offset (same instant as `judged_at` / `launch_timestamp`). |
| `gap_minutes` | `launch_time − event_time` in minutes; negative if the coin launched before the event post. `null` if either side is `null`. |
| `one_off` | `true` iff the narrative was classified as 一次性突发事件. |
| `applied` (also called `snipe` in the P0 draft) | `true` iff all six conditions below hold. |
| `penalty` | −3 if `applied` is true, otherwise 0. |

All six conditions must hold before subtracting 3 (after every cap and floor, before Dev):

1. `one_off` is true (one-off sudden events only; 持续性叙事 and 有明确日期的外部催化 are not “event snipes” — a release date announced months earlier does not count).
2. `event_time` and `launch_time` are both verified, not `null`.
3. `gap_minutes` falls in **[−10, +10]** (inclusive). Negative values cover launching slightly before the event post (e.g. Lemon `FsFqQGxCNd7dbFuimCW4hMiFRdvvM8PoLyp4WZrxAUnN`, about −4 minutes).
4. `event_time` is that event's **first verifiable public post**, not later media pickup, and not the post that merely names the coin.
5. For a re-ignition signal, judge again at the re-ignition moment: if the revival is riding a new one-off event, still run this check.
6. If `event_time` cannot be verified: `applied` is false, `penalty` is 0, `gap_minutes` is `null`. Do not subtract 3 just because it “looks like a snipe.”

The window is ±10 minutes, not ±30. “Within minutes” means a few minutes. Every already-applied snipe with a recorded gap fell inside 5 minutes. A 30-minute window would pull in cases such as a first news post hours earlier and a local-TV reprint 26 minutes before launch, which is not the same as sniping the event.

Examples (do not rescore these; finals stay as logged):

- Applied (still apply under this rule): ZELLANA `4221BwRAHoDiWDiimzL5rCqgYRfKvKdWyWohcLgV3CH1` (~28s); BANKLESS `EcVKznh4T4mfSLggp7QWbvXiKicy3Ev9f3nk3kqHpump` (~16s); Tomás `5ChyeKeH1Hv47qYd13E8ujg8n5Hn3YYPZtLS9DAUpump` (spoiler first post 18:16:27, launch recorded 18:17, ~0.5–1 min — use the spoiler post, not the 18:00 game-open time); GEKKO `GYZ3toP3xAc7xgHfnGBc419ksvtX1w8oP6Tc2bVupump` (~5 min after the official acquisition post); Lemon (launch ~4 min before the People post).
- One-off but **no** snipe: Fiona `B7ncNzYdp71UxqmMQZFL8o2idEZdC1ix9W79J1t8pump` — first cited news post is USA Today 2026-11-17 20:43:47, launch 23:31, gap **+167 minutes** (local TV 23:05 is +26 min; the naming post 23:30:31 is not the event post). Final stays **34**. WHISKEY `G9aeorcqs3CEm2k6rG1XEEmLKRKdofue33p6wXK6pump` (+92 min). Dior `6uVyTB5kkht8kgWpSGBWPJsYczuMcwrsd1Haa2mNpump` (ChanelRion post is ~24h earlier; a naming post 1 minute before launch is not the event post).
- Not one-off, so no snipe even if almost simultaneous with a post: Huss `CUg7AzWL5p8imZVqqdjEPgRhUsXAkBMW5JxkSJ9fpump`; CRIME `4JzrYs2Hwors4bzZpDv2US1Xj7nNvsDvQQ7JKwDXWsDa`; maxi `EBCpGdXuS34WZuX13thrku2KmuLSe3Tf2gMtMKndpump`; COGE `23GBezAJzK8fJNZzgA5LWLyz2WxDNTBfMKtgTQNPpump`.

In the Chinese copy, include a 狙击检查 line with these six values. In JSON, put them under the `snipe` object.

### Dev risk adjustment (apply after every cap and floor above; approved 2026-11-17, partial-data rule approved 2026-11-18)
This is a user-approved exception: the six sub-scores stay strictly ex-ante narrative. Dev data is structural fact already knowable at launch (deployer history, first-10s supply, insiders, self-dump habit), applied once as a penalty on the already-capped total. Do not change any sub-score with it. Always apply this after caps 1–7 (and the event-snipe −3) have already been applied.

Penalties are tiered by the capped narrative total BEFORE this penalty (user rule 2026-11-17: hot narratives are always spotted first by serial launchers, so a busy dev must not sink a good narrative; dev risk matters most when the narrative is weak):
- Pre-penalty total ≥ 55 (A, B, good C): ignore the serial-launcher signals (risk rule 1: launch counts / 24h launches / low graduation rate) entirely. Re-classify using only rules 2–4 (self-dump, first-10s supply / insiders, curve bought out): 高 −3; 中, 低, or 数据不全 → 0. Still show the full Dev block, and note in the copy that serial-launch counts were not penalized because the narrative is strong.
- Pre-penalty total 45–54 (weak C): 高 −6, 中 −2, 低 0, 数据不全 0.
- Pre-penalty total < 45 (D/F): 高 −10, 中 −3, 低 0, 数据不全 0.

If there is no data (non-pump.fun skip, script failure, or unusable output), do not subtract; set `dev.risk` and `dev_penalty` to null, and in the Telegram copy write「Dev 数据未取到」. If some fields exist but the row is 数据不全, set `dev.risk` to `数据不全`, `dev_penalty` to 0, and write「Dev 数据不全，不扣分」.

`narrative_score_100` is the total after caps and then this penalty (do not go below 0). The letter grade (A ≥85, B 70–84, C 45–69, D 30–44, F <30) is assigned to that post-penalty total.

Platform tokens are scored normally (rule 5), so the dev adjustment applies to them like any coin.

Risk rules, using as-of-launch fields from the script (percentages are 0–100; missing values are null, never invented):

**高** if any of these hold (a null field never triggers 高 by itself; do not fill 0 or “looks empty” to make a threshold):
1. `launches_24h` ≥ 5, or `launches` ≥ 50 and graduation rate < 3%.
2. `fast_dump_pct` ≥ 50 (share of the deployer's own coins over the last ~10 days sold within 10 seconds).
3. `first10s_supply_pct` ≥ 50, or `insider_pct` ≥ 20.
4. `curve_bought_at_launch.yes` is true (the bonding curve was bought out at create).

**低** if all of these hold:
1. `launches` ≤ 5 (this coin included) and `launches_24h` ≤ 1.
2. `first10s_supply_pct` < 30 and `insider_pct` < 10. Both must be real numbers; if either is null, 低 cannot be assigned.
3. There is no own-coins record of dumping within 10 seconds on half or more of those coins (`fast_dump_pct` is null or < 50). A null `fast_dump_pct` still satisfies this third clause (do not tighten it).

**中** only when the present numeric fields are enough to rule out both 高 and 低 (for example launches = 32 already fails 低, and dump/concentration fields are complete but below the 高 thresholds). Example: SOYOUARE `GbGzZQugMJtZnhcnNyHUamXg2xGZw4C4RWy1uSvVpump` (32 launches, `fast_dump_pct` 47.8, `first10s_supply_pct` 31.3, `insider_pct` 0) is 中, not 数据不全.

**数据不全**: a needed field for deciding 低 vs 中 is missing, and no present field has triggered 高. Write `dev.risk` as `数据不全` and `dev_penalty` as 0. Do **not** default this to 中.

Classification order: first decide which risk rules are in force for the pre-penalty tier (≥55 → rules 2–4 only; otherwise rules 1–4). Then: (1) if any present numeric field hits a 高 threshold under those rules → 高; (2) else if every field required for 低 that may not be null is present and all 低 clauses pass → 低 (≥55 ignores rule 1's launch-count conditions and only checks the dump/concentration conditions of rules 2–3, with rule 4 not triggered); (3) else if present numbers already rule out both 高 and 低 → 中; (4) else → 数据不全.

Field mapping from `devcheck.py` (read only the script output; if a source field is missing, write null; never type a percentage by hand):

| Report field | Formula | Source in the script output |
|---|---|---|
| `fast_dump_pct` | `deployer.own_coins.out_within_10s_share × 100` | Prefer `own_coins` on the deployer record; otherwise `now.own_coins` on the launch record. The share is over that deployer's own coins in the tables' newest ~10 days. If the whole `own_coins` object is null, this field is null. |
| `first10s_supply_pct` | `bundle_at_creation.supply_share × 100` | `launch.bundle_at_creation.supply_share` (0–1). This is combined supply taken by buyers in the first 10 seconds (0–100). Do not treat `buyers_first_10s` (a headcount) as this percentage. |
| `insider_pct` | `insider_supply_share × 100` | `launch.insider_supply_share` (0–1). |

Other mappings are unchanged: `deployer` ← `deployer.address` (full address); `launches` / `graduations` / `launches_24h` always use the **as-of-launch** `launch.deployer` record (`launches_last_day`), not later-updated totals; graduation rate is computed only when both numbers exist and `launches ≠ 0`; `curve_bought_at_launch.yes` uses the script field of the same name.

Example for the ≥55 tier (rules 2–4 only): 11/17 (The Great Blackout) `FS174V1XfEBAtgF9SBQe3e6NYAoFc5T1c7wKAUjzpump` had a 57 pre-penalty total, 9999 prior launches and 26 in 24h (rule 1, ignored at this tier) but `fast_dump_pct` 92.5% still triggers 高 on rule 2, penalty −3, final 54 C. Do not rescore it.

Grades: A ≥85, B 70–84, C 45–69, D 30–44, F <30.

## Calibration notes (labeled examples, narrative-only)
- Gold examples scored ~70–85 under the old weights: celebrity no-CA meme mapped to a daily ritual name (CZ 'Soon' → GM), official game IP text (GTA6 'Dacity'; historical only — under the 2026-11-19 B decision, official IP text does not count as naming, so cap 1 applies and this pattern no longer reaches 70–85), official film IP calendar re-ignition (Doctor Doom).
- Junk examples: celebrity one-off event sniped with initials ticker (Ronaldo 'CR') — caps 1–3 push it to ~D; callout-account onboarding of a small TikTok creator — D.
- Grassroots memes (cat pfp + catchphrase, DOGE+PEPE) land in C at launch; a later big-account re-ignition is scored separately at that moment.
- Gold (金狗, 2026-11-14): Stand With Jensen $SWJ, BSC Flap, `0x26040e441a7369ccc7bd8bfe96f596df4db87777`, paired with NVDAB (tokenized NVIDIA stock). After Nvidia pushed Jensen Huang out, Lex Fridman (5.58M followers, 3.7M views) literally posted "I stand with Jensen", and Gavin Baker (370K) amplified the standwithjensen.com open letter minutes before launch; the phrase was rising in the hour before launch. First scored 69 (C top, 17/13/13/9/14/3) when a Solana SWJ launched earlier counted as later-same-name (uniqueness ≤4). The user approved the same-chain-only rule (cap 3), so the rescore is 73 (B, uniqueness 7), matching the 金狗 label.
- Junk (垃圾盘, 2026-11-16): BN币安 $NB, BSC, `0xbca41e58dc9282964c43c9a5b565e280e4c1ffff`, pair created 2026-11-09 14:40 UTC+7. Origin: Elon Musk's one-off "Chinese 𝕏 is 牛逼" (25.6M views), which does not name the coin; the community mapped 牛逼 = NB = BN reversed (Binance). At least 4 other NB coins launched on BSC the same day, earlier. First scored 50 C (12/9/8/6/13/2) because cap 2 was skipped and the stack rule did not exist; that disagreed with the 垃圾盘 label. With cap 2 enforced (renewal 6, timing 10), uniqueness 0 (cap 3) and the stack cap 6, it is 40 (D), matching the label.

- Junk batch (垃圾盘, 2026-11-17, weak market): of 15 pump-and-die coins, D-grade and platform tokens were already right; $VI 55 and $WORLDCRAFT 53 were too high only because existing caps were not enforced (rule 7 now makes them ~44 and ~40 D); $ZLUSD 49 and $ZELLANA 49 stay C because the extra rules that would cap them were rejected. $WHISKEY 57 C (first mover, slogan in a 391K-view origin) stays C: later clones are post-launch facts. User guidance: in a bad market many decent narratives die anyway, so do not over-tighten; extra caps for official-product namesakes, celebrity-quip followers and IP pile-ups were rejected.
- 2026-11-18 clarifications do not change logged finals. Fiona `B7ncNzYdp71UxqmMQZFL8o2idEZdC1ix9W79J1t8pump` stays 34 D (event first post is USA Today, +167 min, no snipe). Tomás stays 30 D as a one-off spoiler (not a hard-date IP coin).

## Learned narrative patterns
- Celebrity meme-chain second leg: big account posts an adjacent meme without the CA; community maps it onto an existing coin; a peer quotes later. Counts as renewal. A big account directly posting the CA or adopting the pfp is stronger source strength.
- External IP calendar run-up: obscure coin adopted as concept coin for an upcoming film/game; near-daily official promo re-feeds it (high renewal).
- Riding a hot CT conversation helps timing, but a generic phrase named by a tiny account scores low on source and uniqueness.
- Celebrity one-liner mapped by wordplay (e.g. 牛逼 → NB → BN) spawns many same-name clones on the same chain within hours; a late clone of such a mapping is junk regardless of the origin's view count.

## Short-horizon catalyst (last N hours)
When asked why it just pumped: hourly CA counts to mark spikes; `search_posts_all` from window start; list posts with UTC+7 times and URLs; separate new catalyst vs leftover; say whether big accounts named the CA or only adjacent memes; reject unverified listing claims.

## Output
1. 2–3 Chinese sentences: narrative score + grade (the post-penalty total), then the decisive narrative reasons (mention the event class, any cap/floor applied, the snipe check result, and the dev penalty if any). In the body, also include a 狙击检查 line: event_time, launch_time, gap_minutes, one_off, applied, penalty.
2. JSON (`scoring_version` is always `"v1.0"`. New time fields use ISO 8601 with a numeric offset such as `+07:00`. `dev.risk` may be `高` / `中` / `低` / `数据不全` / `null`):
```json
{
  "ticker": null,
  "contract": null,
  "scoring_version": "v1.0",
  "judged_at": "launch or re-ignition time, UTC+7",
  "narrative": null,
  "event_class": "持续性叙事|有明确日期的外部催化|一次性突发事件",
  "narrative_source": [{"handle": null, "followers": null, "url": null, "time_utc7": null, "engagement": null, "names_coin": null}],
  "sub_scores": {"source_strength": 0, "meme_clarity": 0, "renewal_potential": 0, "cultural_stickiness": 0, "timing_attention": 0, "uniqueness": 0},
  "caps_applied": [],
  "snipe": {
    "event_time": null,
    "launch_time": null,
    "gap_minutes": null,
    "one_off": false,
    "applied": false,
    "penalty": 0
  },
  "origin_not_found": false,
  "dev": {"deployer": null, "launches": null, "graduations": null, "launches_24h": null, "fast_dump_pct": null, "first10s_supply_pct": null, "insider_pct": null, "risk": "高|中|低|数据不全|null"},
  "dev_penalty": null,
  "comment": null,
  "narrative_score_100": 0,
  "grade": "A|B|C|D|F",
  "tags": [],
  "vs_prior_labels": null,
  "confidence": 0,
  "as_of": "YYYY-MM-DD HH:MM UTC+7"
}
```
3. If labeled by the user: agree / disagree / one-line rubric patch.

### Mandatory self-checks before sending (approved 2026-11-23)
1. **`names_coin` needs a quote.** `names_coin: true` only when a pre-launch post literally contains the ticker (e.g. `$ZOL`) or explicitly calls for / names this coin. Quote those exact words in the report. A post that merely uses the word, an IP/product name, or the meme phrase is `false`. When `false`, state in the text whether cap 1 and cap 6 trigger.
2. **DexScreener same-name check every report.** Run `curl -s "https://api.dexscreener.com/latest/dex/search?q=<ticker>"` and also search by name; filter same `chainId` and same symbol/name; list every earlier CA with its `pairCreatedAt` (UTC+7; note it may be the graduation time, not launch). Decide cap 3 / cap 6 from this list and write "no earlier same-name coin found" explicitly when none.
3. **Logging format.** `as_of` is exactly `YYYY-MM-DD HH:MM UTC+7` (never ISO with offset). `base_score` = total after all caps and the snipe check, before the dev penalty; it must equal the sum of sub-scores after caps (minus snipe), and `narrative_score_100` = `base_score` + `dev_penalty`.

## Daily feed workflow
Score each CA narrative-only, one report per CA; contrast table only when asked. Propose rubric patches in chat; rewrite this skill only when the user agrees.

## Anti-scope
No orders, targets, position sizes, or price calls. Do not invent post counts or handles.
