# MicroDecon-in-R
Written by Sneha Sundar, decontaminate phyloseq object in R using microDecon package

microDecon uses the following variables: *grouping_var* (useful if you have sequences from different runs), *NTC_label* (label used to identify the blanks).

You should add those variables to the metadata of your phyloseq object.

For example :
```
# All the samples are from the same run
metadata <- metadata %>%
  mutate(Run = "1")

# The samples will be called "Sample" and the blanks "NTC"
metadata <- metadata %>%
  mutate(Experiment = case_when(
    startsWith(Temps, "T") ~ "Sample",
    .default = "NTC"
  ))
```
And an example of running the function:
```
# The phyloseq object is called "ps"
ps <- phyloseq_remove_contaminants_microDecon(ps, grouping_var = "Run", sample_type_var = "Experiment", NTC_label = "NTC", 
                                              taxa_info = TRUE, taxa_level = "Species", facet_plot = "Nature", taxa_plot = "Genus")

# You can visualize the contaminants presence in the samples using a barplot
ps$diagnostic.plot

# Keep only the cleaned data
ps <- ps$physeq.decontaminated
```
