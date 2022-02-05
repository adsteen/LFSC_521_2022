Lecture 4: Data Visualization with ggplot2
================

``` r
library(tidyverse)
```

# Resources

-   [Healey](https://socviz.co/lookatdata.html#what-makes-bad-figures-bad),
    particularly on [“What makes bad figures
    bad”](https://socviz.co/lookatdata.html#what-makes-bad-figures-bad)
-   [Wilke](https://clauswilke.com/dataviz/)
-   Tufte - see PDFs on Canvas

# Using ggplot2

How does the grammar work? Build a plot up, layer by layer.

``` r
library(tidyverse)

p <- ggplot(data = diamonds) + 
  geom_point(aes(x = carat, y = price, color = cut),
             alpha = 0.5) + 
  #geom_point(aes(x=carat, y=price), color = "green")
  geom_smooth(aes(x = carat, y = price, color = cut), method = "lm") + 
  scale_color_brewer(palette = "Set1") + 
  #scale_color_manual(values = c("black", "red", ...))
  facet_wrap(~cut, nrow = 1) +# 
  #facet_grid(color ~ cut)
  theme_classic() + 
  theme(axis.text.x = element_text(size = 17),
        panel.background = element_rect(color = "red", fill = "darkgreen"))
print(p)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Lecture_4_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
ggsave("my_plot.png", height = 3, width = 4, units = "in", dpi = 300)
```

    ## `geom_smooth()` using formula 'y ~ x'
