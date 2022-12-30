library(ggplot2)

library(data.table)

library(ggthemes)

df <-
  fread(
    "E:\\R-Course-HTML-Notes\\R-Course-HTML-Notes\\R-for-Data-Science-and-Machine-Learning\\Training Exercises\\Capstone and Data Viz Projects\\Data Visualization Project\\Economist_Assignment_Data.csv"
  )


print(head(df))

![data-viz-1](https://user-images.githubusercontent.com/97744709/210115869-9a143963-30d9-4fd5-bf59-0020522755f5.jpg)


#Creating a scatter plot with geom_point()

pl <- ggplot(df, aes(x = CPI, y = HDI)) + geom_point(aes(color = Region))

print(pl)

![data-viz-2](https://user-images.githubusercontent.com/97744709/210116112-0d1f8475-35ad-4e51-901f-6f18bc769d12.jpg)




# pl <-
#   ggplot(df, aes(x = CPI, y = HDI)) + geom_point(shape = 1, size = 5, aes(color =
#                                                                             Region))
##Change the points to be larger empty circles.
##(
##  You'll have to go back and add arguments to geom_point() and reassign it to pl.)
##You'll need to figure out what shape = and size =


# pl <-
#   ggplot(df, aes(x = CPI, y = HDI)) + geom_point(shape = 1, size = 5, aes(color =
#                                                                             Region)) + geom_smooth(aes(group = 1))
##Add geom_smooth(aes(group = 1)) to add a trend line


# pl <-
#   ggplot(df, aes(x = CPI, y = HDI)) + geom_point(shape = 1, size = 5, aes(color =
#                                                                             Region)) + geom_smooth(
#                                                                               aes(group = 1),
#                                                                               method = 'lm',
#                                                                               formula = y ~ log(x),
#                                                                               se = FALSE,
#                                                                               color = 'red'
#                                                                             )
##We want to further edit this trend line.
##Add the following arguments to geom_smooth (outside of aes):method = 'lm' formula = y ~ log(x) se = FALSE color = 'red'


# pl <-
#   ggplot(df, aes(x = CPI, y = HDI)) + geom_point(shape = 1, size = 5, aes(color =
#                                                                             Region)) + geom_smooth(
#                                                                               aes(group = 1),
#                                                                               method = 'lm',
#                                                                               formula = y ~ log(x),
#                                                                               se = FALSE,
#                                                                               color = 'red'
#                                                                             ) + geom_text(aes(label = Country, color = Region))
##It's really starting to look similar! But we still need to add labels, we can use geom_text!
##Add geom_text(aes(label=Country)) to pl2 and see what happens.
##(Hint: It should be way too many labels)


pl <-
  ggplot(df, aes(x = CPI, y = HDI)) + geom_point(shape = 1, size = 5, aes(color =
                                                                            Region)) + geom_smooth(
                                                                              aes(group = 1),
                                                                              method = 'lm',
                                                                              formula = y ~ log(x),
                                                                              se = FALSE,
                                                                              color = 'red'
                                                                            )

pointsToLabel <-
  c(
    "Russia",
    "Venezuela",
    "Iraq",
    "Myanmar",
    "Sudan",
    "Afghanistan",
    "Congo",
    "Greece",
    "Argentina",
    "Brazil",
    "India",
    "Italy",
    "China",
    "South Africa",
    "Spane",
    "Botswana",
    "Cape Verde",
    "Bhutan",
    "Rwanda",
    "France",
    "United States",
    "Germany",
    "Britain",
    "Barbados",
    "Norway",
    "Japan",
    "New Zealand",
    "Singapore"
  )

pl2 <- pl + geom_text(
  aes(label = Country),
  color = "gray20",
  data = subset(df, Country %in% pointsToLabel),
  check_overlap = TRUE
)
##Labeling a subset is actually pretty tricky ! So we're just going to give you the answer since it would require manually selecting the subset of countries we want to label!


# pl3 <- pl2 + theme_bw()
##Almost there ! Still not perfect, but good enough for this assignment.
##Later on we'll see why interactive plots are better for labeling.
##Now let's just add some labels and a theme, set the x and y scales and we're done!
##Add theme_bw() to your plot and save this to pl4


# pl3 <-
#   pl2 + theme_bw() + scale_x_continuous(name = "Corruption Perception Index, 2011 (10 = least corrupt)",
#                                         limits = c(1, 10),
#                                         breaks = 1:10)
##Add scale_x_continuous() and set the following arguments:name = Same x axis as the Economist Plot
##limits = Pass a vector of appropriate x limits
##breaks = 1:10


# pl3 <-
#   pl2 + theme_bw() + scale_x_continuous(name = "Corruption Perception Index, 2011 (10 = least corrupt)",
#                                         limits = c(1, 10),
#                                         breaks = 1:10) + scale_y_continuous(name = "Human Development Index, 2011 (1 = best)", limits =
#                                                                               c(0.2, 1))
##Now use scale_y_continuous to do similar operations to the y axis!


pl3 <-
  pl2 + theme_bw() + scale_x_continuous(name = "Corruption Perception Index, 2011 (10 = least corrupt)",
                                        limits = c(1, 10),
                                        breaks = 1:10) + scale_y_continuous(name = "Human Development Index, 2011 (1 = best)", limits =
                                                                              c(0.2, 1)) + ggtitle("Corruption and Human Development") + theme(plot.title = element_text(hjust =
                                                                                                                                                                           0.5))
##Finally use ggtitle() to add a string as a title.


print(pl3)
