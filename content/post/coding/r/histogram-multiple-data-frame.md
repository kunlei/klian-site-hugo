---
title: Recipe - Plotting Multiple Histograms in one Graph
date: 2020-07-23
draft: false
tags: ["R"]
---

I recently had a small data analysis work that required showing two histograms in the same graph.
It has been quite a while since my I last used R in my work, and I had to resort to google to find the solution (see the code below).

```r
# create two sample data frames
x <- data.frame(x = sample(0 : 10, 100, TRUE), group = 'Group A')
y <- data.frame(x = sample(5 : 20, 50, TRUE), group = 'Group B')

# bind them together
data <- rbind(x, y)

# make the histogram using ggplot
ggplot(data, aes(x, fill = group, colour = group)) +
  geom_histogram(aes(y = ..density..), breaks=seq(0, 20, 1), alpha=0.6, 
                 position="identity", lwd=0.2)
```

The graph looks like this:

![graph](/img/coding/r-histogram-multi-data-frame.png)
