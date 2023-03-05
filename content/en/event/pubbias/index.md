---
title: "Controlling for Publication Bias: Challenges & Future Directions"

event: ESMARConf2023
event_url: https://esmarconf.github.io/index.html

summary: Panel discussion at the 2023 ESMARConf conference.
abstract: "The goal of systematic reviews and meta-analyses is to provide a comprehensive, unbiased synthesis of the available evidence in a research field. This aim is seriously threatened if we have reasons to believe that some results are systematically missing or underrepresented in the published literature. Controlling adequately for such publication biases in meta-analyses remains challenging. Various methods are available, which differ in their assumptions concerning why publication bias arises, as well as how it manifests itself. Bringing together highly experienced field experts, the goal of this panel discussion is to highlight current state-of-the-art methods to control for publication bias, and their implementations in R. We also want to shed light on how evidence synthesists may navigate the great variety of approaches and implementations, and if some may be preferable to others. Finally, we aim to explore open research questions and future directions in the development of methods to adjust for publication bias."

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
publishDate: '2023-03-27T13:00:00Z'
# date_end: '2030-06-01T15:00:00Z'
all_day: false

# Schedule page publish date (NOT talk date).
publishDate: '2023-03-04T13:00:00Z'

authors: [harrer]
tags: [meta-analysis, R]

# Is this a featured talk? (true/false)
featured: false

aliases:
- /talk-esmarconf23-en

links:
- icon: twitter
  icon_pack: fab
  name: "@MathiasHarrer"
  url: https://twitter.com/MathiasHarrer
# url_code: ""
# url_pdf: "https://drive.google.com/file/d/1hgsEvbcnHA4ZG4TizzZSV1arojeKON8C/preview"
# url_slides: ""
# url_video: "https://youtu.be/b-FJ9GnrXRQ"

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
---

<style>
b {
  font-family: Roboto;
  font-weight: bold;
}
</style>


<h3>Further Reading</h3>

<hr>

<b>Mathur, M. B.</b> (2022, June 1). <i>Sensitivity analysis for $p$-hacking in meta-analyses.</i> https://doi.org/10.31219/osf.io/ezjsx

Maier, M., VanderWeele, T.J., <b>Mathur, M.B.</b> (2022). Using selection models to assess sensitivity to publication bias: A tutorial and call for more routine use. <i>Campbell Systematic Reviews.</i>  https://doi.org/10.1002/cl2.1256

<b>Mathur, M. B.</b>, & VanderWeele, T. J. (2020). Sensitivity analysis for publication bias in meta-analyses. <i>Journal of the Royal Statistical Society. Series C, Applied Statistics, 69</i>(5), 1091. https://doi.org/10.1111/rssc.12440

<b>Bartoš, F.</b>, Maier, M., Wagenmakers, E. J., Doucouliagos, H., & Stanley, T. D. (2021). Robust Bayesian meta-analysis: Model-averaging across complementary publication bias adjustment methods. <i>Research Synthesis Methods</i>. https://doi.org/10.1002/jrsm.1594

<b>Bartoš, F.</b>, & Schimmack, U. (2022). $Z$-curve 2.0: Estimating replication rates and discovery rates. <i>Meta-Psychology, 6</i>. https://doi.org/10.15626/MP.2021.2720

<b>Bartoš, F.</b>, Gronau, Q. F., Timmers, B., Otte, W. M., Ly, A., & Wagenmakers, E. J. (2021). Bayesian model-averaged meta-analysis in medicine. <i>Statistics in Medicine</i>. https://doi.org/10.1002/sim.9170

<b>Bartoš, F.</b>, Maier, M., Wagenmakers, E.-J., Nippold, F., Doucouliagos, H., Ioannidis, J. P. A., Otte, W. M., Sladekova, M., Fanelli, D., & Stanley, T. D. (2022). <i>Footprint of publication selection bias on meta-analyses in medicine, economics, and psychology</i>. https://doi.org/10.48550/arXiv.2208.12334

<b>Page, M.J.</b>, Sterne, J.A.C., Boutron, I., Hróbjartsson, A., ..., Higgins, J.P.T. (2020). <i>Risk Of Bias due to Missing Evidence (ROB-ME): a new tool for assessing risk of non-reporting biases in evidence syntheses</i> (Version 24 October 2020) https://sites.google.com/site/riskofbiastool//welcome/rob-me-tool.

<b>Page, M.J.</b>, Sterne, J.A.C., Higgins, J.P.T., Egger, M. (2021). Investigating and dealing with publication bias and other reporting biases in meta-analyses of health research: a review. <i>Research Synthesis Methods</i>, 12(2):248-259. https://doi.org/10.1002/jrsm.1468

<b>van Aert, R. C. M.</b> & van Assen, M. A. L. M. (2022). <i>Correcting for publication bias in a meta-analysis with the $p$-uniform* method</i>. https://doi.org/10.31222/osf.io/zqjr9

<b>van Aert, R. C. M.</b>, Wicherts, J. M., & van Assen, M. A. L. M. (2016). Conducting meta-analyses on $p$-values: Reservations and recommendations for applying $p$-uniform and $p$-curve. </i>Perspectives on Psychological Science, 11</i>(5), 713-729. https://doi.org/10.1177/1745691616650874

<br>

<h3>Selected Functions & Packages in R</h3>

<hr>

Braginsky M, <b>Mathur M</b> (2023). phacking: Sensitivity Analysis for p-Hacking in Meta-Analyses. R package version 0.1.0, https://CRAN.R-project.org/package=phacking.

Braginsky M, <b>Mathur M</b>, VanderWeele T (2023). PublicationBias: Sensitivity Analysis for Publication Bias in Meta-Analyses. R package version 2.3.0, https://CRAN.R-project.org/package=PublicationBias.

<b>Bartoš F</b>, Maier M (2020). “RoBMA: An R Package for Robust Bayesian Meta-Analyses.” R package version 2.3.1, https://CRAN.R-project.org/package=RoBMA.

<b>van Aert RC</b> (2022). puniform: Meta-Analysis Methods Correcting for Publication Bias. R package version 0.2.5, https://CRAN.R-project.org/package=puniform.

<b>Viechtbauer W</b> (2023). selmodel: Selection Models. https://wviechtb.github.io/metafor/reference/selmodel.html

<br>


