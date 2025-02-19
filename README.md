##New Coder Surver: Job Role Preference Versus Age
-   [**Creating the Shiny
    Application**](#creating-the-shiny-application)
    -   [**Building the User Interface**](#building-the-user-interface)
    -   [**Building the Server Logic**](#building-the-server-logic)

[New Coder
Dataset](https://www.kaggle.com/freecodecamp/2016-new-coder-survey-)

**Creating the Shiny Application**
----------------------------------

First we need to load the libraries:

    require("RCurl")
    require("dplyr")
    require("tidyr")
    require("ggplot2")
    require("ggthemes")
    library("shiny")
    library("shinydashboard")

### **Building the User Interface**

    shinyUI(fluidPage(
    tabItem(tabName = "plot",
            sliderInput("age", "Age of New Coder: ", min = 15, max = 60, value = 25),
            mainPanel(
              tabsetPanel(
                tabPanel("Dot Plot", plotOutput("plot")), 
                tabPanel("Summary", verbatimTextOutput("summary"))
              )))))

### **Building the Server Logic**

**Step 1** Import the csv file:

    df<-read.csv("NewCoders.csv",stringsAsFactors = FALSE)

**Step 2** Make the output react to the input, assigning it to the
`renderPlot()` function:

`output$plot <-renderPlot()`

**Step 3** Filter the data from the original dataset using the dplyr
package:

    filtered <-df %>%
            dplyr::select(AGE,JOBROLEINTEREST) %>%
            dplyr::filter(AGE==input$age, JOBROLEINTEREST != "null")%>%
            dplyr::group_by(AGE,JOBROLEINTEREST)%>%
            dplyr::summarize(count = n())%>%dplyr::arrange(desc(count))

**Step 4** Build the ggplot output within the `renderPlot()` function:

    ggplot(filtered,aes(x= filtered$JOBROLEINTEREST, y = filtered$count, fill=filtered$JOBROLEINTEREST))  + geom_bar(stat="identity")+ theme(axis.text.x = element_text(angle = 90, hjust = 1)) +theme(legend.position="none")+ylim(0,150)

![](Shiny_bargraph.png)

#### **Interpretation of Bar Graph**

Here we have Age as input in the slider bar. Notice that as Age
increases, the job role preferences change significantly. In particular,
a majority of the new coders in the 18 to 35 years age range prefer to
become Full-Stack Web Developers or Front-End Web Developers. Beyond the
age of the 35 the sample size decreases and the Job Role Interests
becomes more evenly distributed.
