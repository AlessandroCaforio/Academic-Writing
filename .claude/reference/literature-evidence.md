# Literature Evidence Knowledge Base

> Extracted from RAG corpus (105 papers) + web searches. Organized thematically for use by `/write-introduction`, `/write-section`, and the `narrative-architect` agent.
>
> **Format**: Each entry has: *Paper* | *Finding* | *Magnitude* (if available) | *Method* | *Use in thesis*

---

## 1. THE TASK-BASED FRAMEWORK

### 1.1 The Original Framework: ALM (2003)

**Paper**: Autor, Levy & Murnane (2003) "The Skill Content of Recent Technological Change" *QJE*

- **Core idea**: Technology doesn't replace "skills" wholesale — it substitutes for *routine tasks* (both cognitive and manual) while complementing *non-routine* tasks (analytical, interactive, manual-physical).
- **Key taxonomy**: Routine cognitive (bookkeeping), routine manual (assembly), non-routine cognitive analytical (research), non-routine cognitive interactive (management), non-routine manual physical (janitorial).
- **RTI formula**: RTI = (routine_cog + routine_man) - (nr_cog_anal + nr_man_phys)
- **Data**: Dictionary of Occupational Titles (DOT) task measures.
- **Key quote** (p.1283): "In our usage, a task is 'routine' if it can be accomplished by machines following explicit programmed rules."
- **Use in thesis**: Foundation for identifying routine occupations (top 1/3 by RTI) in our RCA calculation. Forward ref to Ch.3 O*NET measures.

### 1.2 Labor Market Polarization: Autor & Dorn (2013)

**Paper**: Autor & Dorn (2013) "The Growth of Low-Skill Service Jobs" *AER*

- **Finding**: Employment polarization — growth at top (abstract) and bottom (service) of skill distribution, hollowing out of the middle (routine).
- **Magnitude**: Middle-skill occupations declined substantially in routine-intensive commuting zones.
- **Method**: CZ-level analysis using DOT/O*NET task measures + Census employment data.
- **IV**: Uses historical industry structure as Bartik instrument.
- **Use in thesis**: Establishes that automation's labor market effects are concentrated in routine middle-skill occupations — the groups we study.

### 1.3 Automation and Wage Inequality: Acemoglu & Restrepo (2022)

**Paper**: Acemoglu & Restrepo (2022) "Tasks, Automation, and the Rise in US Wage Inequality" *Econometrica*

- **Finding**: Automation accounts for 50-70% of changes in the US wage structure between 1980-2016.
- **Method**: Task model with industry-level BEA data, 49 industries, automation proxies (robots, software, dedicated equipment).
- **Key definition**: "Automation technologies as any technology that enables machines or algorithms to perform tasks previously allocated to humans (which thus leads to the displacement of workers from these tasks)."
- **Data**: BEA Integrated Industry-Level Production Accounts — same source as our td measure.
- **Use in thesis**: Direct methodological ancestor. Our td_{i,t,k} replicates their industry labor share approach with rolling windows.

### 1.4 The Task Displacement Measure: Acemoglu, Restrepo & Kong (2024/2025)

**Paper**: Acemoglu, Restrepo & Kong (2024) "Tasks At Work" NBER WP 32872; Acemoglu & Restrepo (2025) "Automation and Rent Dissipation"

- **Finding**: Task displacement captures actual displacement mechanism, not just skill composition.
- **Method**: Industry labor share change regressed on automation proxies (robots, software, equipment). Predicted component = automation-driven TD. Residual = other factors.
- **Group-level formula**: TD_g = Σ_i ω_{gi} × RCA_{gi} × td_i (our thesis follows this exactly)
- **Key insight**: Rent dissipation — automation doesn't just displace workers, it erodes the rents (above-market wages) that workers in automatable jobs enjoyed.
- **Magnitudes**: "The five industries with the highest and lowest changes in labor shares" show dramatic variation (Figure 4 in A-R 2025).
- **Use in thesis**: PRIMARY methodology reference. Our measure is a rolling-window adaptation of their cumulative approach.

### 1.5 Automation Theory Review: Restrepo (2024)

**Paper**: Restrepo (2024) "Automation: Theory, Evidence, and Outlook" *Annual Review of Economics*

- **Summary**: Comprehensive review establishing that (i) task models explain labor trends better than SBTC, (ii) displacement effects of automation are empirically supported, (iii) routine middle-skill occupations most affected.
- **Key passage**: "Automation technologies operate by substituting capital for labor in a widening range of tasks. This substitution reduces costs, creating a positive productivity effect, but also reduces employment opportunities for workers displaced from automated tasks, creating a negative displacement effect."
- **Use in thesis**: Authoritative review supporting our task-based approach.

---

## 2. LABOR SHARE DECLINE

### 2.1 Superstar Firms: Autor et al. (2020)

**Paper**: Autor, Dorn, Katz, Patterson & Van Reenen (2020) "The Fall of the Labor Share and the Rise of Superstar Firms" *QJE*

- **Finding**: Labor share decline concentrates in industries with rising concentration — "superstar firms" with high productivity capture market share, reducing average labor share.
- **Mechanism**: Between-firm reallocation, not within-firm compression.
- **Connection to A-R**: Acemoglu, Lelarge & Restrepo (2020) show French manufacturing confirms this — firms adopting automation expand at competitors' expense.
- **Use in thesis**: Supports using industry-level labor share change as TD proxy. Also motivates markup_exposure control variable.

### 2.2 Global Labor Share Decline: Karabarbounis & Neiman (2014)

**Paper**: Karabarbounis & Neiman (2014) "The Global Decline of the Labor Share" *QJE*

- **Finding**: Labor share declined in most countries and industries since ~1980. ~50% explained by falling relative price of investment goods.
- **Magnitude**: Global labor share fell ~5pp over 35 years.
- **Mechanism**: Cheaper capital → substitution of capital for labor in production.
- **Use in thesis**: (i) Motivates using labor share decline as automation proxy (ii) Supports European IV strategy (if implemented) — global trends mean EU labor share changes capture similar automation forces.

---

## 3. ECONOMIC SHOCKS → POLITICAL ATTITUDES

### 3.1 The China Shock and Politics: Autor, Dorn, Hanson & Majlesi (2020)

**Paper**: ADHM (2020) "Importing Political Polarization" *AER*

- **Finding**: CZs with larger Chinese import exposure → more polarized political outcomes. Both parties moved to extremes.
- **Magnitude**: Import exposure significantly increased Republican vote share; moved exposed districts toward more ideologically extreme candidates.
- **Method**: Shift-share (Bartik) IV — uses industry employment shares × change in Chinese imports. Instruments US imports with imports to other developed countries.
- **Key quote**: "The fraction L_{jkt}/L_{jt} is the share of industry k in CZ j's total employment, as measured in County Business Patterns data prior to the outcome period."
- **Identification**: Pre-period industry shares × exogenous Chinese supply shock.
- **Use in thesis**: Methodological template for our shift-share approach. Our ω_{gi} × td_i parallels their employment share × import shock. Also motivates china_exposure control.

### 3.2 Trade and Economic Nationalism (Europe): Colantone & Stanig (2018)

**Paper**: Colantone & Stanig (2018) "The Trade Origins of Economic Nationalism" *AJPS*

- **Finding**: Import competition from China → increased support for nationalist/radical-right parties in 15 European countries.
- **Method**: District-level + individual-level with NUTS-2 import shock. Country-year FE. IV: imports to other countries.
- **Key mechanism**: Import shock → economic nationalism, not just anti-incumbent voting. "The effects extend to voting for parties with nationalist platforms emphasizing worker protection."
- **Use in thesis**: Key precedent for economic shock → rightward political shift. Supports the cultural backlash mechanism (it's not just economic policy preferences).

### 3.3 Automation and the 2016 Election: Frey, Berger & Chen (2018)

**Paper**: Frey, Berger & Chen (2018) "Political Machinery: Did Robots Swing the 2016 US Presidential Election?" *OREP*

- **Finding**: US counties with higher robot exposure voted significantly more for Trump.
- **Magnitude**: Trump gained on average **1.309 percentage points** of two-party vote share for each unit increase in robot exposure (preferred IV estimate).
- **Counterfactual**: If robot exposure had been 10% lower, Michigan would have swung to Clinton. At 95% lower, Pennsylvania and Wisconsin also flip → Clinton wins Electoral College.
- **Method**: County-level OLS + IV (European robot adoption as instrument).
- **Use in thesis**: Dramatic illustration of automation → political consequences. CZ-level; our approach adds demographic group variation.

### 3.4 Automation and Voting in Europe: Anelli, Colantone & Stanig (2019)

**Paper**: Anelli, Colantone & Stanig (2019) "We Were The Robots" *IZA DP*

- **Finding**: Higher robot exposure → increased support for **nationalist and radical-right parties** in 14 Western European countries (1993-2016).
- **Transmission channels**: Robot exposure → poorer perceived economic conditions, lower satisfaction with government/democracy, reduced political self-efficacy.
- **Method**: Both regional-level (shift-share) and individual-level (based on demographic characteristics × pre-sample employment). IV: robot adoption in OTHER countries.
- **Key magnitude**: Individual exposure → +3.5pp radical-right support (IV estimate).
- **Robustness**: Effect survives controls for value orientations (arguably post-treatment) and Oesch social class — "even comparing two individuals with the same value orientations, those more exposed to automation support nationalist options."
- **Note**: Authors are Bocconi (same as thesis supervisor Anelli).
- **Use in thesis**: Closest methodological precedent. Our individual-level → group-level approach builds on their individual exposure design.

### 3.5 AI Shock and Political Attitudes: Kurer & Palier (2024)

**Paper**: Kurer & Palier (2024) "The Artificial Intelligence Shock and Socio-Political Attitudes" *Tech. Forecasting & Social Change*

- **Finding**: Bifurcation — automation-susceptible workers are more culturally conservative + economically left-leaning. AI-complemented workers are socially liberal + fiscally conservative.
- **Key insight**: "Both cohorts are increasingly voting based on their non-economic values" — voting AGAINST their economic interests.
- **Method**: ANES survey data (1980-2016), 26,311 Americans. Fixed effects OLS, clustered at occupation level. Two proxies: (i) automation potential (Manyika et al. 2017), (ii) AI exposure.
- **Use in thesis**: Directly supports our hypothesis. Also supports using IRT ideology (continuous) rather than binary vote choice.

### 3.6 Automation and Openness: Im et al. (2023)

**Paper**: Im, Koivula, Kuhn & Seki (2023) "Automation versus Openness" *Comparative Political Studies*

- **Finding**: Both trade exposure and automation risk shape electoral preferences, but through different channels.
- **Cited in**: Gallego & Kurer (2022) review.
- **Use in thesis**: Additional evidence for technology-specific political effects (not just general economic anxiety).

---

## 4. MECHANISMS: WHY ECONOMIC SHOCKS → CONSERVATIVE SHIFT

### 4.1 The Gallego & Kurer (2022) Review — KEY SYNTHESIS

**Paper**: Gallego & Kurer (2022) "Automation, Digitalization, and AI in the Workplace: Implications for Political Behavior" *Annual Review of Political Science* 25:463-484

- **Central question**: "Is there something special about workplace digitalization vis-à-vis other structural changes?"
- **Answer**: Yes — different economic transformations have different political consequences depending on whether and how they are perceived by voters and politicized by political actors.
- **Three mechanisms identified**:
  1. **Misattribution/diversion**: Workers blame immigrants/trade rather than technology
  2. **Authoritarian aggression**: "Members of historically dominant groups develop authoritarian attitudes as a protection from social regression and identity loss stemming from long-run economic change, and they turn against groups perceived as inferior in order to preserve status" (citing Gidron & Hall 2017, Ballard-Rosa et al. 2022)
  3. **Social identity switching**: "When individuals experience relative economic decline, their occupation-related identities become less valuable, and they become more likely to choose identities that can provide higher prestige and self-esteem, such as national identities" (citing Shayo 2009)
- **Open question**: "The relative importance of the different channels remains unknown."
- **Use in thesis**: Authoritative framing. Our thesis can speak to mechanism 2 (authoritarian shift visible in IRT ideology) and 3 (identity switching from occupational to national).

### 4.2 Cultural Backlash Thesis: Inglehart & Norris (2016, 2019)

**Paper**: Inglehart & Norris (2016) "Trump, Brexit, and the Rise of Populism"; Norris & Inglehart (2019) *Cultural Backlash*

- **Thesis**: Populist support reflects a cultural backlash against progressive value change (post-materialism), not primarily economic grievances.
- **Evidence**: In Europe, Model C (demographics + cultural attitudes) provides a more parsimonious account of populist voting than economic variables alone.
- **Key finding**: "The only major change to the economic variables is that the effects of economic insecurity reverse direction" when combined with cultural attitudes.
- **However**: "Populist support can only be attributed to a combination of economic insecurity AND authoritarian values" — interaction effect.
- **Class voting decline**: Class voting index fell from ~50 (Sweden 1948) to ~26 by 1990. "A substantial share of the working class has shifted their support to populist parties."
- **Use in thesis**: Establishes cultural backlash as competing explanation. Our IRT measure captures the value dimension they emphasize.

### 4.3 Economic Grievances → Authoritarian Values: Norris & Inglehart (2019) Ch.5

**Paper**: Norris & Inglehart (2019) *Cultural Backlash* Chapter 5: Economic Grievances

- **Finding**: Both authoritarianism and populism are "generally significantly stronger among the less prosperous respondents" — working class, low-income, manufacturing sector, income insecurity, economic dissatisfaction.
- **But**: "When the strength of the standardized betas are compared, important differences can be observed across both cultural dimensions."
- **Exceptions**: "Indicators of direct experience of long-term unemployment and dependency upon state benefits were in the reverse direction to that expected."
- **Authoritarian values are more enduring**: "Authoritarian values seem to be more enduring than political [populist] attitudes" — less sensitive to economic fluctuations.
- **Use in thesis**: Nuances the mechanism. Economic shocks activate pre-existing authoritarian predispositions rather than creating them de novo.

### 4.4 Status Threat: Mutz (2018) and Morgan (2018) Debate

**Paper**: Mutz (2018) "Status Threat, Not Economic Hardship, Drove the 2016 Presidential Vote" *PNAS*

- **Claim**: Status threat (perceived decline in dominant group standing), not pocketbook economic concerns, drove Trump support.
- **Mechanism**: "It's not a threat to their own economic well-being; it's a threat to their group's dominance in our country over all."
- **Key finding**: After accounting for perceived status threat, "even lack of a college education no longer predicts Trump support."
- **Method**: Panel data (2012-2016) with individual fixed effects.

**Paper**: Morgan (2018) "Status Threat, Material Interests, and the 2016 Presidential Vote" *Socius* (CRITIQUE)

- **Counter**: Morgan reanalyzes Mutz's data and finds "changes in status threat do not explain away changes in material interests as predictors of changes in relative thermometer ratings."
- **Issue**: Mutz's full model includes arguably endogenous variables (party ID, candidate distance measures) that "rob predictive power" from other variables.
- **Conclusion**: "Table 4 does not support Mutz's conclusion that there is 'overwhelming' evidence that status threat is the sole or even the primary explanation."
- **Use in thesis**: Present BOTH sides. Our approach avoids the Mutz vs. Morgan debate by using continuous IRT ideology rather than binary Trump/Clinton vote, and by measuring actual economic exposure (TD) rather than subjective perceptions.

### 4.5 Social Status and Populist Right: Gidron & Hall (2017)

**Paper**: Gidron & Hall (2017) "The Politics of Social Status: Economic and Cultural Roots of the Populist Right" *BJS*

- **Finding**: Declining *subjective* social status among low-educated men → increased support for populist right parties. In ALL but 2 of 12 countries, relative social status of men without college education is lower today than 25-30 years ago.
- **Mechanism**: "Lower levels of subjective social status are associated with voting for the populist right."
- **Gender contrast**: Women's subjective social status ROSE over the same period — explaining why populist right appeal is gender-differentiated.
- **Use in thesis**: Supports our demographic group design (Edu × Gender × Age). Status decline is GROUP-level, matching our unit of analysis.

### 4.6 The Declining Middle: Kurer (2020)

**Paper**: Kurer (2020) "The Declining Middle: Occupational Change, Social Status, and the Populist Right" *CPS* 53:10-11, 1798-1835

- **Finding**: Semiskilled routine workers in the lower middle class bear the brunt of employment structure transformation. Perception of *relative* economic decline (not impoverishment) drives support for conservative and especially radical-right populist parties.
- **Key insight**: "The strongest political reaction may be observed among those who are MOST CONCERNED about their economic well-being and future prospects rather than among the hardest-hit."
- **Mechanism**: Gradual preference change → later electoral decision-making. Not sudden switch but slow drift.
- **Use in thesis**: Supports lagged TD effects (our k=1,2,3 window). Also supports why we see stronger effects at shorter lags.

### 4.7 Identity, Beliefs, and Political Conflict: Bonomi, Gennaioli & Tabellini (2021)

**Paper**: Bonomi, Gennaioli & Tabellini (2021) "Identity, Beliefs, and Political Conflict" *QJE*

- **Model**: When economic shocks undermine occupational identity, people switch to cultural/national identity. This shift distorts beliefs and increases demand for conservative social policies.
- **Key mechanism**: "Identity depends on which social partition is salient... political identities can be highly persistent when conflict-creating shocks are long-lasting."
- **Prediction**: Trade/automation shock in exposed districts → (i) more conservative social policies from BOTH parties, (ii) LESS redistributive Democratic policy, (iii) more conservative propaganda from Republicans.
- **Use in thesis**: Theoretical foundation for why automation → conservative (not leftward). Economic decline triggers identity switching to national/cultural identity → rightward ideology.

### 4.8 Identity Politics: Gennaioli & Tabellini (2025)

**Paper**: Gennaioli & Tabellini (2025) "Presidential Address: Identity Politics" *Econometrica*

- **Extension**: Formal model showing economic shocks can trigger cultural polarization through identity switching.
- **Key result**: "A rise in η [economic shock] that causes some voters to switch to cultural identity" → candidates from both parties announce more conservative social policies, reducing divergence on redistribution while increasing divergence on cultural issues.
- **Use in thesis**: Provides the theoretical micro-foundations for our empirical finding (β > 0).

### 4.9 Economic Decline and Authoritarianism: Ballard-Rosa, Jensen & Scheve (2022)

**Paper**: Ballard-Rosa, Jensen & Scheve (2022) "Economic Decline, Social Identity, and Authoritarian Values in the United States" *ISQ*

- **Finding**: Sustained economic decline negatively affects social identity of historically dominant groups → increased authoritarian values → "force minority groups to conform to social norms as compensation for identity losses."
- **Key result**: Individuals in diverse regions facing Chinese import competition show MORE authoritarian values. The diversity × economic shock interaction matters.
- **Use in thesis**: Supports the identity-threat mechanism. Also shows why geographic variation (our state-level spec) matters.

### 4.10 Social Identity and Redistribution: Shayo (2009)

**Paper**: Shayo (2009) "A Model of Social Identity with an Application to Political Economy" *APSR* 103:147-174

- **Model**: People identify with groups that are (i) similar to them AND (ii) dissimilar from outgroup. Economic decline → occupation-based identity becomes less valuable → switch to national identity.
- **Three predictions**: (i) National identification more common among the poor, (ii) national identification reduces support for redistribution, (iii) across democracies, more national identification = less redistribution.
- **Use in thesis**: Explains the PARADOX of automation losers moving rightward (not demanding more redistribution). Key theoretical reference for the contribution section.

### 4.11 Psychological Foundations: Osborne et al. (2023)

**Paper**: Osborne et al. (2023) "The Psychological Causes of Authoritarianism" *Nature Reviews Psychology*

- **Dual Process Model**: Two distinct ideological attitudes — right-wing authoritarianism (RWA) and social dominance orientation (SDO).
- **Key finding**: "Social — but not economic — threats activate right-wing authoritarianism." Threats → preference for conformity, tradition, security → prejudice toward specific outgroups.
- **Personality correlates**: RWA correlated with need for order, low openness to experience.
- **Use in thesis**: Supports cultural backlash mechanism. Economic shock → perceived social threat → authoritarian activation → conservative shift.

### 4.12 Identity Threat Integration: Manunta et al. (2025)

**Paper**: Manunta et al. (2025) "Identity Threat Mediates Both Economic and Cultural Paths" *PSPB*

- **Finding**: Identity threat serves as a common mediator for BOTH economic and cultural pathways to political conservatism.
- **Integration**: Resolves the economic vs. cultural debate — both operate through identity threat.
- **Use in thesis**: Theoretical keystone. Explains why our single ideology measure captures effects from both economic and cultural channels.

### 4.13 Populism — Demand and Supply: Guiso et al. (2017)

**Paper**: Guiso, Herrera, Morelli & Sonno (2017) "Populism: Demand and Supply"

- **Finding**: Economic insecurity drives demand for populist policies, but "frustration-induced abstention" is also key — insecure voters may not vote at all.
- **Mechanism**: "Cultural sentiments, such as distrust for traditional politics and attitudes towards immigrants, are key drivers of the populist vote albeit indirectly so" — they MEDIATE the economic insecurity effect.
- **Use in thesis**: Supports indirect mechanism (economic shock → cultural attitudes → political change).

---

## 5. METHODOLOGICAL FOUNDATIONS

### 5.1 Shift-Share (Bartik) Identification: Borusyak, Hull & Jaravel (2022)

**Paper**: BHJ (2022) "Quasi-Experimental Shift-Share Research Designs" *ReStat*

- **Framework**: Shift-share estimands can be interpreted as shift-based (exogeneity of shocks) or share-based (exogeneity of shares).
- **Conditions for validity**: Under the shifts-based approach, identification requires (i) many sectors, (ii) exogenous shocks, (iii) limited correlation between shocks and shares.
- **Application**: ADH (2013) setting — Chinese import competition on CZ employment.
- **Use in thesis**: Justifies our shift-share TD design. Industry-level td shocks are plausibly exogenous to group-level political attitudes.

### 5.2 Shares-Based Identification: Goldsmith-Pinkham, Sorkin & Swift (2020)

**Paper**: GPSS (2020) "Bartik Instruments: What, When, Why, and How" *AER*

- **Alternative**: Treats the shares (not shifts) as the source of identifying variation.
- **Implication**: Pre-period industry employment shares must be exogenous to future political trends.
- **Use in thesis**: Our shares (ω_{gi}^{(t-k)}) are from ACS at time t-k, predetermined relative to outcomes at time t.

### 5.3 Pseudo-Panel Design: Deaton (1985)

**Paper**: Deaton (1985) "Panel Data from Time Series of Cross-Sections" *JoE*

- **Method**: When individual panel data unavailable, use cohort (demographic group) means from repeated cross-sections.
- **Requirement**: Cohorts must be defined by time-invariant characteristics.
- **Cell sizes**: Larger cells → more precise group means → less measurement error.
- **Use in thesis**: Justifies our 18-group pseudo-panel from CCES repeated cross-sections. Minimum cell size: 501 observations.

---

## 6. CONTRADICTORY/NUANCED FINDINGS (for honest discussion)

### 6.1 No Rage Against the Machines: Zhang (2019)

**Paper**: Zhang (2019) "No Rage Against the Machines: Threat of Automation Does Not Change Policy Preferences" *MIT Working Paper*

- **Finding**: Automation threat does NOT significantly change policy preferences in experimental settings.
- **Use in thesis**: Important null finding. Difference between stated experimental preferences and revealed electoral behavior.

### 6.2 Automation Losers Don't Turn Authoritarian: Buzzelli, Nicoli & Sacchi (2025)

**Paper**: Buzzelli, Nicoli & Sacchi (2025) "Automation and Political Realignment" *Political Studies*

- **Finding**: Against expectations, "no significant authoritarian turn among routine workers." Job loss → stronger pro-redistribution stances. Career upgrading → libertarian/laissez-faire orientation.
- **Method**: Long-run panel data (BHPS 1991-2008), structural equation models (ISSP).
- **Use in thesis**: Important counter-evidence. UK context (not US). May differ because UK political landscape offers different channeling options. Discuss in limitations.

### 6.3 Restrict Foreigners, Not Robots: Wu (2021)

**Paper**: Wu (2021) "Restrict Foreigners, Not Robots: Partisan Responses to Automation Threat" *Working Paper*

- **Finding**: Partisan framing matters — automation threat is not equally politicized as trade/immigration. Workers may not attribute job loss to technology.
- **Use in thesis**: Supports the misattribution mechanism from Gallego & Kurer (2022). Also explains why our effect (β=0.291) is moderate.

---

## 7. ARGUMENTATIVE FLOW TEMPLATES

### 7.1 Introduction: The Paradox

```
FLOW: Automation displaces workers → Economic models predict leftward shift (demand for redistribution)
      → BUT empirical evidence shows RIGHTWARD shift → WHY?
      → Identity threat / cultural backlash mechanism → This thesis tests this with task displacement
```

### 7.2 Literature Review §2.1: From Tasks to Displacement

```
FLOW: ALM (2003) establishes task framework
      → Autor & Dorn (2013) documents polarization
      → A-R (2022) quantifies wage inequality effects
      → A-R-K (2024/2025) creates task displacement measure
      → THIS THESIS adopts rolling-window version of A-R-K
```

### 7.3 Literature Review §2.2: From Economic Shocks to Politics

```
FLOW: ADH (2013) establishes trade shock → labor market effects
      → ADHM (2020) shows trade shock → political polarization
      → Colantone & Stanig (2018) confirms in Europe: trade → nationalism
      → Frey et al. (2018) shows robots → Trump vote
      → Anelli et al. (2019) shows robots → radical right in Europe
      → GAP: No one has used TASK DISPLACEMENT (not just robots/trade) for politics
      → THIS THESIS fills this gap
```

### 7.4 Literature Review §2.3: The Mechanism Debate

```
FLOW: Economic insecurity thesis (Inglehart & Norris 2016, Guiso et al. 2017)
      → Cultural backlash thesis (Norris & Inglehart 2019)
      → Status threat (Mutz 2018) vs material interests (Morgan 2018)
      → RESOLUTION via identity switching:
        • Gidron & Hall (2017): declining status → populist right
        • Shayo (2009): economic decline → national identity → less redistribution
        • Bonomi et al. (2021): identity switching → conservative social policy
        • Kurer (2020): relative decline (not absolute) matters most
        • Manunta et al. (2025): identity threat integrates both channels
      → THIS THESIS: IRT ideology captures BOTH economic and cultural dimensions
```

### 7.5 Contribution Structure

```
4 CONTRIBUTIONS:
1. MEASURE: Task displacement (not just RTI/robot counts) — captures actual displacement
2. OUTCOME: IRT ideology (continuous, both dimensions) — not binary vote choice
3. DESIGN: Pseudo-panel 2006-2022 with 18 groups — not single cross-section
4. MECHANISM: Can adjudicate between insecurity vs backlash via heterogeneous effects
```

---

## 8. PAPERS NOT IN RAG — CONSIDER ADDING TO references.bib

| Paper | Key for | Status |
|-------|---------|--------|
| Karabarbounis & Neiman (2014) | Labor share decline | Need bib entry |
| Barkai (2020) | Profit share rise | Need bib entry |
| Borusyak, Hull & Jaravel (2022) | Shift-share inference | Need bib entry |
| Goldsmith-Pinkham, Sorkin & Swift (2020) | Shares-based Bartik | Need bib entry |
| Kurer (2020) | Declining middle | Need bib entry |
| Gallego & Kurer (2022) | Review article | Need bib entry |
| Shayo (2009) | Social identity model | Need bib entry |
| Bonomi, Gennaioli & Tabellini (2021) | Identity politics QJE | Need bib entry |
| Gennaioli & Tabellini (2025) | Identity politics Econometrica | Need bib entry |
| Ballard-Rosa, Jensen & Scheve (2022) | Economic decline + authoritarianism | Need bib entry |
| Buzzelli, Nicoli & Sacchi (2025) | Null finding on auth. turn | Need bib entry |
| Zhang (2019) | Null finding on preferences | Need bib entry |
| Guiso, Herrera, Morelli & Sonno (2017) | Populism demand/supply | Need bib entry |
| Im et al. (2023) | Automation vs openness | Need bib entry |
| Kurer & Palier (2024) | AI shock + attitudes | Need bib entry |
