---
# Title, summary, and page position.
linktitle: "Sampling Variances"
weight: 2
icon_pack: fas
date: "2022-01-25"

# Page metadata.
title: "Sampling Variances"
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

<object data="/media/workshop/ma/sampling-variances.pdf" type="application/pdf" width="100%" height="500px">
</object>

**Keywords**: Sampling Error, Estimand, Standard Error, Precision


<br></br>

## Code 

---

<br>

Sampling From A Larger Distribution:

{{< highlight R >}}
set.seed(123)
sample <- rnorm(n = 50, mean = 10, sd = 2)
mean(sample)
{{< /highlight >}}

<br>

Generating A Sampling Distribution:

{{< highlight R >}}
# Step 1
means <- vector()
for (i in 1:100)
    means[i] <- mean(rnorm(n = 50, mean = 10, sd = 2))
hist(means, breaks=20)

# Step 2
means <- vector()
for (i in 1:1000)
    means[i] <- mean(rnorm(n = 50, mean = 10, sd = 2))
hist(means, breaks=20)

# Step 3
set.seed(123)
means <- vector()
for (i in 1:10000)
    means[i] <- mean(rnorm(n = 50, mean = 10, sd = 2))
hist(means, breaks=20)

# Calculate SD
sd(means)
{{< /highlight >}}

<br>

Using the Standard Error Formula:

{{< highlight R >}}
sd(sample)/sqrt(50)
{{< /highlight >}}


<style>
h1 {color: #2a7792;}
</style>


