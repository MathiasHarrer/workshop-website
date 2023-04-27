---
# Title, summary, and page position.
linktitle: "The Equal-Effects Model"
weight: 2
icon_pack: fas
date: "2022-01-25"

# Page metadata.
title: "The Equal-Effects Model"
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

<object data="/media/workshop/ma/equal.pdf" type="application/pdf" width="100%" height="500px">
</object>

**Keywords**: Fixed-Effect Model, Common-Effect Model


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

# Check if this worked
data


# Equal-Effect Model (Inverse-Variance)
# Calculate the inverse variance-weights for each study
data$w <- 1/data$se^2

# Then, we use the weights to calculate the pooled effect
pooled_effect <- sum(data$w*data$es)/sum(data$w)
pooled_effect
{{< /highlight >}}


<style>
h1 {color: #2a7792;}
</style>


