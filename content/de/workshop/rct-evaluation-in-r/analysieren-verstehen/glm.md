---
# Title, summary, and page position.
linktitle: "Generalisierte Lineare Modelle"
weight: 6
date: "2021-01-25"

# Page metadata.
title: "Generalisierte Lineare Modelle"
type: book  # Do not modify.
---

<style>
code{
  color: #2a7792;
}
.hljs{
  font-size: 14px
}



</style>

---

## Slides

---

<iframe src="https://drive.google.com/file/d/117bRqbY9wjSy8M7jakcYDjBxtYTMldmF/preview" width="640" height="480" allow="autoplay"></iframe>


<br></br>

## Praxis-Teil

---


### Logistische Regression

{{< highlight R >}}
# Wir fokussieren hier auf die logistische Regression.
# Wir nutzen die neu generierte Reliable Improvement-Variable als Repsonse

# In einem Set:
m.logreg <- glm(ri ~ 1 + group + pss.0, data = implist[[1]],
                family = binomial("logit"))
summary(m.logreg)


# In den MI-Sets:
with(implist, glm(ri ~ 1 + group + pss.0,binomial("logit"))) %>%
  testEstimates() -> mi.logreg
mi.logreg
{{< / highlight >}}
