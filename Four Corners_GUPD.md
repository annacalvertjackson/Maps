
![](https://i.imgur.com/Oow7aQf.png)
# Map of the Four Corners


##Map of Four Corners
##Map of Arizona


##Clear R Memory

rm(list=ls())


##Libraries I need:
library(ggplot2)
library(ggmap)
library(maps)
library(mapdata)

##Base states data

states<- map_data("state")

##Map of USA

ggplot(data = states) + 
  geom_polygon(aes(x = long, y = lat, fill = region, group = group), color = "white") + 
  coord_fixed(1.3) +
  guides(fill=FALSE)  # do this to leave off the color legend

##Map of Four Corners

four_corners<- subset(states, region %in% c("utah", "colorado", "new mexico", "arizona"))

four_corners_map<-ggplot(data = four_corners) + 
  geom_polygon(aes(x = long, y = lat, group=group), fill = "palegreen", color = "black") +
  coord_fixed(1.3) + ##This keeps the scale pretty
  theme(axis.text.x = element_blank(), ##These get rid of axis ticks/text
        axis.text.y = element_blank(),
        axis.ticks = element_blank())
        
four_corners_map_blank<- four_corners_map + theme(plot.title = element_blank(),
                        axis.title.x = element_blank(),
                        axis.title.y = element_blank()) ##These get rid of axis titles

four_corners_map_blank

##Map of Arizona

arizona<- subset(states, region %in% c("arizona"))

map_arizona<-ggplot(data = arizona) + 
  geom_polygon(aes(x = long, y = lat, group=group), fill = "palegreen", color = "black") +
  coord_fixed(1.3)

map_arizona

##Read in data points for contemporary sites in Arizona

"C:/Users/annac/Desktop/Prairie Dogs/Datasets"

contemporary<-read.csv("AZ_Contemporary Sites.csv")

head(contemporary)

##map the contemporary points

ggplot() +
  geom_point(data=contemporary, aes(x=Longitude, y=Latitude), color="red") ##Color has to be specified outside of aes()

##combine contemporary points onto the Arizona map

ggplot(data = arizona) + 
  geom_polygon(aes(x = long, y = lat, group=group), fill = "palegreen", color = "black") +
  coord_fixed(1.3) +
  geom_point(data=contemporary, aes(x=Longitude, y=Latitude), color="red") +
  theme(legend.position = "none") ##this gets rid of legend

##Read in data points for museum sites in Arizona

"C:/Users/annac/Desktop/Prairie Dogs/Datasets"

museum<-read.csv("AZ_Museum Sites.csv")

head(museum)

##map the museum points

ggplot() +
  geom_point(data=museum, aes(x=Longitude, y=Latitude), color="deepskyblue")

##Combine map of Arizona with data points from BOTH contemporary AND museum sites


arizona_all_sites<-ggplot(data = arizona) + 
       geom_polygon(aes(x = long, y = lat, group=group), fill = "palegreen", color = "black") +
       coord_fixed(1.3) +
       geom_point(data=contemporary, aes(x=Longitude, y=Latitude), color="red") +
       geom_point(data=museum, aes(x=Longitude, y=Latitude), color="deepskyblue")

arizona_all_sites
      
  
##Inset four corners map into arizona map
  
library(cowplot)

Inset_Map<-ggdraw() +
    draw_plot(arizona_all_sites) +
    draw_plot(four_corners_map_blank, x = 0.05, y = 0.65, width = 0.3, height = 0.3)
 

Inset_Map  
  
