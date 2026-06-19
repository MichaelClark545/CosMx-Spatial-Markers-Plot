# CosMx-Spatial-Markers-Plot
This script allows you to put transcript markers out of the probe set in accordance to the tissue morphology. No data standardization or adjustments have been made. This is a good landmark and tissue anatomy identifier tool. All code is written in R and requires the CosMx SMI data to be a Seurat Object. Additional markers can be added based on the users needs with repeat code of a new marker from lines 23-42 and a new geom point in lines 59-64. 

```{r}
#Install and Load Packages
install.packages("Seurat")
install.packages("ggplot2")
install.packages("pheatmap")
library(Seurat)
library(pheatmap)   # Heatmap visualization
library(ggplot2)# General plotting
library(dplyr)
library(tidyr)
```

```{r}
#Pull Seurat Object off Downloads
Seurat_Object = readRDS("File_Path")

```

```{r}
#Pull Marker 1 Data 
Marker_1 <- GetAssayData(
  Seurat_Object,
  assay = "SCT",
  layer = "data"
)["Marker_1", ]

```

```{r}
#Plot Marker 1 Data
plot_df <- data.frame(
  x = Seurat_Object_1$x_slide_mm,
  y = Seurat_Object_1$y_slide_mm,
  Marker_1 = as.numeric(Marker_1),
)
```

```{r}
plot_df$Marker_1_pos <- plot_df$Marker_1 > 0
```

```{r}
#Obtain Final Spatial Plot with Selected Marker and Tissue Morphology
ggplot() +

  # Background tissue
  geom_point(
    data = plot_df,
    aes(x = x, y = y),
    color = "black",
    size = 0.05,
    alpha = 0.3
  ) +

  # Marker_1
  geom_point(
    data = subset(plot_df, Marker_1_pos),
    aes(x = x, y = y),
    color = "red",
    size = 0.1
  ) +

  coord_fixed() +
  theme_classic() +
  labs(
    title = "Marker_1 (red)"
  )
```
