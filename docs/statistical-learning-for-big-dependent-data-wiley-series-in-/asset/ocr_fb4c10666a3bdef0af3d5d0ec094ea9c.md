```
### S-T plots
### Figure 9.1
>library(sp)
>library(gstat)
>data(meuse)
>data(meuse.riv)
>plot(meuse.riv,type="l",asp=1,xlab="meuse.x",ylab="meuse.y",
main="Meuse river")
>points(meuse$xy,meuse$y,pch="x",cex=0.6)
### Figure 9.3
>require(fields)
>data(ozone2)
> good <- !is.na(ozone2$y[16,]) # day 16 and remove missing values
> out <- as.image(ozone2$y[16,good],x=ozone2$lon.lat[good,])
> image.plot(out)
> map("state",xlim=range(ozone2$lon.lat[good,1]),
ylim=range(ozone2$lon.lat[good,2]),add=T)
> map.text("state",xlim=range(ozone2$lon.lat[good,1]),
ylim=range(ozone2$lon.lat[good,2]),cex=1,add=T)
> points(ozone2$lon.lat[good,])
#
```

```
#library("animation")
>require("dplyr")
>require("ggplot2")
>require("maps")
>require("STbook")
>set.seed(1)
>data("NOAA_df_1990",package="STRbook") ## Load data
>Tmax=filter(NOAA_df_1990,proc=="Tmax" & month %in% 5:9 & year==1993)
>Tmax %>% select(lon,lat,date,julian,z) %>% head()
```

```
>Tmax$t <- Tmax$Julian - 728049
>Tmax_1 <- subset(Tmax, t %in% c(2,16,31))
>NOAA_plot<-ggplot(Tmax_1)+geom_point(aes(x=lon,y=lat,colour=z),size=2)+
col_scale(name = "degF")+xlab("Longitude (deg)")+
ylab("Latitude (deg)") +
geom_path(data = map_data("state"), aes(x=long,y=lat,group=group)) +
facet_grid(~date)+coord_fixed(xlim=c(-105,-75),ylim=c(25,50)) +
theme_bw()
>print(NOAA_plot)
```

```
### Time Series plot
>set.seed(11)
>UIDs <- unique(Tmax$id)
>UIDs_sub <- sample(UIDs,10)
>Tmax_sub <- filter(Tmax, id %in% UIDs_sub)
>TmaxTS <- ggplot(Tmax_sub) +
geom_line(aes(x = t,y=z))+
facet_wrap(~id,ncol=5) +
xlab("Day number (days)") +
ylab("Tmax (degF)") +
theme_bw() +
theme(panel.spacing = unit(1,"lines"))
>print(TmaxTS)
```

# ####### Hovmoller plot