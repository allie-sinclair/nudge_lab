# Nudge Lab

A classroom game about choice architecture. Students pick a mission (café purchasing, retirement saving, blood donation, vegetables at a buffet, flu shots, green energy), choose up to 3 of the 8 nudges available in that mission, and simulate the result against a target. Every simulation takes place in a new setting, so students can hit **Simulate again** to see how much the same nudges vary. A history chart tracks every attempt (hover or tap a point to see the nudges used), and **Reveal the best strategy** shows the research averages behind every nudge.

The average effects are calibrated to **[Mertens et al. (2022, PNAS)](https://doi.org/10.1073/pnas.2107346118)**, using values from the corrected article, with their **moderate publication-bias adjustment** applied by default. The in-app "How it works" page documents the model and its assumptions. A short tutorial appears the first time someone opens the app and can be reopened from the header.

The app is a single `index.html` with no dependencies or backend. Nothing students do is sent anywhere.

## Deploy on GitHub Pages

1. Create a public repository (e.g., `nudge-lab`).
2. Upload `index.html`, `README.md`, and `.nojekyll` to the root of the `main` branch.
3. Go to **Settings → Pages → Build and deployment**, set **Source** to **Deploy from a branch**, choose **main**, folder **/ (root)**, then click **Save**.
4. After a minute or two the site is live at `https://<your-username>.github.io/nudge-lab/`.

To preview locally, open `index.html` in a browser.

## Class presets (URL parameters)

| Parameter | Values | Effect |
|---|---|---|
| `bias` | `none`, `moderate` (default), `severe` | Applies the publication-bias adjustments from Mertens et al.'s sensitivity analysis (×1, ×0.775, ×0.20) |
| `hetero` | `off`, `partial` (default), `full` | Variation between simulations: SD = 0, ½τ, or τ (τ ≈ 0.40 to 0.51) |

Example: `.../nudge-lab/?bias=severe`. Settings on the Missions page builds this link for you.

## Model in brief

- d(technique, domain) = d(category | domain, Fig. 5) × d(technique, Table 2) ÷ d(category, Table 2)
- d is converted to a percentage via ln(OR) = 1.81·d (Chinn, 2000), applied to the scenario baseline. Each simulation draws 5,000 people per group.
- **Targets:** the best package's average outcome under the moderate bias adjustment, rounded down to the nearest 5% (1% for the blood drive via `targetStep`; café and green energy are set by hand via `fixedTarget`). There are 8 nudges per mission and 92 possible packages. Targets: café 58%, retirement 60%, blood drive 27%, buffet 65%, flu shots 65%, green energy 28%. On average, between 1 (blood drive) and 12 (flu shots) packages reach the target. With partial variation, even the best package reaches it in only about 50–65% of runs.
- **Stacking with variation:** the 1, ½, ¼ weights are assigned by each nudge's *average* effect and then applied to that run's random draws, so a package's median simulated outcome equals its average outcome.
- **Game assumptions (not from the literature):** stacked nudges add with weights 1, ½, ¼ (Mertens et al. excluded multi-technique studies). Scenarios and baselines are invented.

## Customizing

Everything is near the top of the `<script>`: `TECHNIQUE`, `DOMAIN`, `BIAS`, `HETERO`, `COMBO_WEIGHTS`, `N_PER_ARM`, and the `MISSIONS` array (copy and baselines). Targets are computed automatically by `computeTarget`.

## References

- Mertens, S., Herberz, M., Hahnel, U. J. J., & Brosch, T. (2022). The effectiveness of nudging: A meta-analysis of choice architecture interventions across behavioral domains. *PNAS, 119*(1), e2107346118. https://doi.org/10.1073/pnas.2107346118 (Correction: https://doi.org/10.1073/pnas.2204059119)
- Maier, M., Bartoš, F., Stanley, T. D., Shanks, D. R., Harris, A. J. L., & Wagenmakers, E.-J. (2022). No evidence for nudging after adjusting for publication bias. *PNAS, 119*(31), e2200300119. https://doi.org/10.1073/pnas.2200300119
- Szaszi, B., et al. (2022). No reason to expect large and consistent effects of nudge interventions. *PNAS, 119*, e2200732119. https://doi.org/10.1073/pnas.2200732119
- Bakdash, J. Z., & Marusich, L. R. (2022). Left-truncated effects and overestimated meta-analytic means. *PNAS, 119*, e2203616119. https://doi.org/10.1073/pnas.2203616119
- Chinn, S. (2000). A simple method for converting an odds ratio to effect size for use in meta-analysis. *Statistics in Medicine, 19*(22), 3127–3131.

This is an independent teaching demo and is not affiliated with the authors above.
