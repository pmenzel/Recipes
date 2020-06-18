# How to use Microsoft fonts (Arial etc.) in ggplot2

## Install Microsoft fonts in Ubuntu

```
sudo apt install ttf-mscorefonts-installer
```

Test if Arial is installed:
```
fc-match Arial                                                                                                                                                                                                             [10:41:05]
```

Output should be:
```
Arial.ttf: "Arial" "Regular"
```

## Prepare fonts in R:
Install `extrafont` package:
```
conda install r-extrafont
```

Load fonts in R, only do once:
```
library(extrafont)
font_import()
loadfonts()
```
Test if Arial is available via `fonttable()` or `fonts()`

## Use in plots:
```
p <- ggplot() + ... + theme(text = element_text(family = "Arial"))
ggsave(p, filename = "out.pdf")
```
There should be no errors from `ggsave()`.

