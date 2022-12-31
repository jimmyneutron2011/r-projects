library(ggplot2)

library(ggthemes)

df <- read.csv('Economist_Assignment_Data.csv')

print(head(df))

![data-viz-1](https://user-images.githubusercontent.com/97744709/210115869-9a143963-30d9-4fd5-bf59-0020522755f5.jpg)



**#Creating a scatter plot with geom_point()**

pl <- ggplot(df, aes(x = CPI, y = HDI)) + geom_point(aes(color = Region))

print(pl)

![data-viz-2](https://user-images.githubusercontent.com/97744709/210116112-0d1f8475-35ad-4e51-901f-6f18bc769d12.jpg)



**#Changing the points to be larger empty circles**

pl1 <- ggplot(df, aes(x = CPI, y = HDI)) + geom_point(shape = 1, size = 5, aes(color = Region))

print(pl1)

![data-viz-3](https://user-images.githubusercontent.com/97744709/210134956-61a39738-418a-4fa7-af03-9094fde7999f.jpg)



**#Adding geom_smooth(aes(group = 1)) to add a trend line**

pl2 <- pl1 + geom_smooth(aes(group = 1))

print(pl2)

![data-viz-4](https://user-images.githubusercontent.com/97744709/210135021-70d42e91-5f9b-46d3-bc42-a1e099985935.jpg)




**##Further editing this trend line**

pl2 <- pl1 + geom_smooth(aes(group = 1), method = 'lm', formula = y ~ log(x), se = FALSE, color = 'red')

print(pl2)

![data-viz-5](https://user-images.githubusercontent.com/97744709/210135108-e8c1d12d-247b-4a81-86bc-8e96724483a9.jpg)



**#Adding labels using geom_text**

pointsToLabel <-c("Russia", "Venezuela", "Iraq", "Myanmar", "Sudan", "Afghanistan", "Congo", "Greece", "Argentina", "Brazil", "India", "Italy", "China", "South Africa", "Spain", "Botswana", "Cape Verde", "Bhutan", "Rwanda", "France", "United States", "Germany", "Britain", "Barbados", "Norway", "Japan", "New Zealand", "Singapore")

pl3 <- pl2 + geom_text(aes(label = Country), color = "gray20", data = subset(df, Country %in% pointsToLabel), check_overlap = TRUE)

print(pl3)

![data-viz-7](https://user-images.githubusercontent.com/97744709/210135569-6b6279cf-598d-4289-a8d6-a7ccefb71b44.jpg)



**#Adding theme_bw(), scale_x_continuous() and scale_y_continuous**

pl4 <- pl3 + theme_bw() + scale_x_continuous(name = "Corruption Perception Index, 2011 (10 = least corrupt)", limits = c(1, 10), breaks = 1:10) + scale_y_continuous(name = "Human Development Index, 2011 (1 = best)", limits =c(0.2, 1))

print(pl4)

![data-viz-9](https://user-images.githubusercontent.com/97744709/210135972-35d70913-73f0-460f-8542-865cb1f0c1c7.jpg)



**#Finally using ggtitle() to add a string as a title**

pl5 <- pl4 + ggtitle("Corruption and Human Development") + theme(plot.title = element_text(hjust = 0.5))

print(pl5)

![data-viz-10](https://user-images.githubusercontent.com/97744709/210136062-45f2915a-126e-4d08-a9c1-c680515a4d80.jpg)
