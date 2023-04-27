---
# Title, summary, and page position.
linktitle: "Between-Study Heterogeneity"
weight: 4
icon_pack: fas
date: "2022-01-25"

# Page metadata.
title: "Between-Study Heterogeneity"
type: book  # Do not modify.
---

<style>
code{
  color: #2a7792;
}
.hljs{
  font-size: 16px
}
.minih{
  font-size: 1px;
  margin: 0px 0px 0px 0px;
}

.highlight {
    position: relative;
}
.highlight pre {
    padding: 15px;
}
.highlight-copy-btn {
    position: absolute;
    top: 7px;
    right: 7px;
    border: 0;
    border-radius: 4px;
    padding: 5px;
    font-size: 0.7em;
    line-height: 1.8;
    color: #fff;
    background-color: #777;
    min-width: 55px;
    text-align: center;
}
.highlight-copy-btn:hover {
    background-color: #666;
}
</style>

---


## Slides {.minih}

<object data="/media/workshop/ma/heterogeneity.pdf" type="application/pdf" width="100%" height="500px">
</object>

**Keywords**: Cochran's _Q_, I-Squared, Prediction Intervals, Outliers, Baujat Plot


<br></br>

## Code

---

{{< highlight R >}}
# This data set contains effects of studies examining
# the effects of suicide prevention interventions
# Copy and paste the entire code below into your Console.
# Then hit Enter.
data <- structure(
            list(
                study = c("Berry et al.", "DeVries et al.", 
                          "Fleming et al.", "Hunt & Burke", 
                          "McCarthy et al.", "Meijer et al.", 
                          "Rivera et al.", "Watkins et al.", 
                          "Zaytsev et al."), 
                es = c(-0.143, -0.608, -0.111, -0.127, -0.392, 
                       -0.268, 0.012, -0.245, -0.126), 
                se = c(0.147, 0.17, 0.258, 0.176, 0.202, 
                       0.135, 0.183, 0.224, 0.194)), 
                row.names = c(NA, -9L), 
            class = "data.frame")

# Random-Effects Model
# If necessary, install the meta package
install.packages("meta")

# Load the meta package
library(meta)

# Using our 'data' dataset, run metagen:
rem <- metagen(es, se, study, data)
summary(rem)
forest(rem)

# Prediction Intervals
# Set prediction to TRUE
rem <- metagen(es, se, study, data, prediction = TRUE)
summary(rem)
forest(rem)

# Outlier Removal
# Exclude study by de Vries (no. 2)
rem <- metagen(es, se, study, data[-2,], prediction = TRUE)
summary(rem)
forest(rem)
{{< /highlight >}}



<style>
h1 {color: #2a7792;}
</style>


