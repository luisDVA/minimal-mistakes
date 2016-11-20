---
published: false
---
Bar plots are a good way to show continuous (or count) data that is grouped into categories. When we have few categories (~4 or fewer), plotting bars side by side is probably the most straightforward and common solution. 

<figure>
    <a href="/images/pointsonly.png"><img src="/images/pointsonly.png"></a>
        <figcaption>just the points in 2d</figcaption>
</figure>


If we have only two categories and we want to show the contrast in values between the two, then diverging ‘stacked’ bar plots (thanks to data scientist Matt Sandy [@appupio](https://twitter.com/appupio) for the terminology) look to be a pretty effective visualization strategy.  

Last month, as part of a group exercise at a workshop, we were plotting some vegetation data with two grouping categories. My suggestion was to plot dodged bars, but marine biologist [Antonio Canepa Oneto](https://www.researchgate.net/profile/Antonio_Canepa) sketched a diverging bar plot on paper and suggested we give that a try. I had never tried to make a plot like this and I couldn’t find any documentation for making such plots in R, but after a few minutes of fiddling with the stat argument in ggplot2 we were able to make a nice figure that really highlighted the differences in values between two groups.

Since then I’ve noticed these types of plots online, mainly in some journalistic figures for topics that involved very dichotomous variables (pro and versus GMO, US elections, etc.).

Here’s some R code to create stacked bar charts using ggplot2. The figure below should be fully reproducible, and it more or less follows the type of plot of plant diversity that inspired this post. 

The following block of code goes through X major steps.
1. Set up some sample data, representing two parallel vegetation transects on different slopes of a ravine in which native vs introduced plants were recorded at five sampling points. This is already in ‘long’ form.
2. Conditionally invert the signs for the values of one of the two categories (in this case multiplying all the introduced species richness values by -1.
3. Plot the bars using stat=”identity” and position=”identity”
4. Re-specify the y axis breaks and labels using the pretty function and the abs function because the values weren’t really negative.
5. Make the figures pretty using the artyfarty package, and use facet wrapping to summarize even more data.
  
{% highlight r %}
# long-form vegetation survey data
# these data should more or less reflect the vegetation patterns at "Quebrada de Cordoba", Chile
library(dplyr)

vegSurvey <- 
data.frame(sampling_point=rep(c(1:5),4),
           slope=c(rep("North",10),rep("South",10)),
           veg_Type=rep(c(rep("native",5),rep("introduced",5)),2),
           spp=as.integer(abs(rnorm(20,5,2))))

vegSurvey$spp <-   ifelse(vegSurvey$veg_Type =="introduced",vegSurvey$spp+1,vegSurvey$spp)

{% endhighlight %}