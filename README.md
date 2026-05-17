# Ethanol & Recovery Calculator

A browser-based tool for estimating blood alcohol concentration (BAC), driving legality under Norwegian traffic law, and physiological recovery time — personalised to your body and drink profile.

---

## Features

- **Personalised BAC estimate** using the Widmark formula, adjusted for ethnic group, biological sex, age, and BMI
- **Norwegian driving law check** — flags whether your estimated peak BAC is above or below the 0.2‰ legal limit, and how long until you drop below it
- **Safe intake threshold** — compares your ethanol intake against health-based recommended limits, with a visual progress bar and risk classification
- **Recovery timeline** — clock-based milestones showing when BAC halves, when you are likely legal to drive, and when ethanol is largely cleared
- **No data sent anywhere** — all calculations run locally in the browser

---

## Inputs

| Field | Description |
|---|---|
| Ethnic group | Asian / European / American — affects alcohol distribution ratio and metabolism rate |
| Biological sex | Male / Female — affects body water ratio and liver metabolism |
| Age | Years — metabolism slows modestly after 60 |
| Weight | kg |
| Height | cm — used to compute BMI |
| Alcohol % | e.g. `40` for spirits, `5` for beer, `12` for wine |
| Volume | ml of liquid consumed |
| Drink duration | How many minutes the drinking session lasted |

---

## Outputs

### Key numbers
- **Total ethanol (g)** — grams of pure ethanol in your drink
- **Peak BAC (‰)** — estimated maximum blood alcohol concentration
- **Hours to clear** — estimated time until ethanol is largely metabolised
- **Hours until legal to drive** — time until BAC drops to or below 0.2‰

### Driving status
Green / red card showing whether your estimated BAC is within or above Norway's legal driving limit of **0.2‰**, and the estimated excess and wait time if over.

### Safe threshold card
Colour-coded risk level (safe / warning / danger) comparing your intake to evidence-based recommended limits, adjusted for your profile:

| Profile | Recommended limit |
|---|---|
| Male, non-Asian | ≤ 40 g ethanol per session |
| Female, non-Asian | ≤ 30 g ethanol per session |
| Male, Asian | ≤ 28 g ethanol per session |
| Female, Asian | ≤ 20 g ethanol per session |

### Recovery timeline
Time-stamped milestones calculated from the moment you hit *Calculate*:
- Peak — current estimated BAC and what to avoid
- Legal threshold — when BAC is estimated to reach 0.2‰
- Halfway point — midpoint of metabolism, with nutrition reminders
- Cleared — when ethanol is largely metabolised

---

## How the calculation works

### Ethanol content
```
ethanol_g = volume_ml × (alcohol_% / 100) × 0.789
```
`0.789` is the density of ethanol in g/ml.

### Peak BAC (Widmark formula)
```
BAC (‰) = ethanol_absorbed / (body_weight_kg × r)
```
Where `r` is the Widmark distribution factor:

| Group | Base r |
|---|---|
| Male | 0.68 |
| Female | 0.55 |

Adjustments applied:
- **Asian** ethnic group: r × 0.92 (lower total body water ratio on average)
- **BMI > 30**: r × 0.95
- **BMI < 20**: r × 1.03

Ethanol metabolised *during* the drinking session is subtracted before computing peak BAC, using the session duration and the individual metabolism rate.

### Metabolism rate
Base rate: **0.15‰ per hour**

Adjustments:
- Asian ethnic group: × 0.80 (higher prevalence of ALDH2 enzyme deficiency)
- Female: × 0.90
- Age > 60: × 0.88

### Time to legal / clear
```
hours_to_legal = (peak_BAC - 0.2) / metabolism_rate   [if peak_BAC > 0.2]
hours_to_clear = peak_BAC / metabolism_rate
```

---

## Legal context — Norway

Norway's Road Traffic Act sets a BAC limit of **0.2‰** — one of the strictest in Europe. Penalties escalate with BAC:

| BAC | Consequence |
|---|---|
| 0.2–0.5‰ | Fine + licence suspension |
| 0.5–1.2‰ | Conditional / unconditional imprisonment |
| > 1.2‰ | Unconditional imprisonment |

---

## Limitations & disclaimer

> **These results are estimates for reference only. Do not use this tool to decide whether it is safe or legal to drive.**

Actual BAC is affected by many factors this model cannot account for:

- Food in the stomach (slows absorption significantly)
- Hydration status
- Medication or other substances
- Individual liver enzyme activity
- Fatigue and sleep deprivation (impairs performance independently of BAC)
- Rate of absorption, which varies day to day

The Widmark formula is a population average. Any individual's real BAC may be meaningfully higher or lower than the estimate. When in doubt, do not drive and allow more time than the calculator suggests.

---

## Data source

Recommended intake thresholds and metabolic guidance are based on:

> *Định mức ethanol an toàn & phục hồi sau uống rượu* (Safe ethanol thresholds & post-drinking recovery — practical guide), a reference document covering ethanol pharmacokinetics, hangover risk thresholds, and recovery nutrition.

Norwegian BAC limits sourced from the Norwegian Road Traffic Act (*Vegtrafikkloven*) § 22.

---

## License

MIT — free to use, modify, and distribute. Attribution appreciated but not required.
