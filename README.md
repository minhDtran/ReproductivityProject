ReproductivityProject
=====================

Productivity Project : compare the MGP between an automatic and manual transmission

This readme file contains the server.R and ui.R codes for generating my shiny web page.

This readme file also introduces to the idea of computing MPG based on the automatic and manual automobile model.

The code for computing it are located in the R mark down and the HTML files.

Code of my server.R for my Shiny app at https://dungminhtran.shinyapps.io/Minh/

library(shiny)
library(datasets)
library(ggplot2) # load ggplot
 
### Define server logic required to plot various variables against mpg
shinyServer(function(input, output) {
### Compute the forumla text in a reactive function since it is
### shared by the output$caption and output$mpgPlot functions
formulaText <- reactive(function() {
paste("mpg ~", input$variable)
})
# Return the formula text for printing as a caption
output$caption <- reactiveText(function() {
formulaText()
})
### Generate a plot of the requested variable against mpg and only
### include outliers if requested
### ggplot version
output$mpgPlot <- reactivePlot(function() {
### check for the input variable
if (input$variable == "am") {
### am
mpgData <- data.frame(mpg = mtcars$mpg, var = factor(mtcars[[input$variable]], labels = c("Automatic", "Manual")))
}
else {
### cyl and gear
mpgData <- data.frame(mpg = mtcars$mpg, var = factor(mtcars[[input$variable]]))
}
 
p <- ggplot(mpgData, aes(var, mpg)) +
geom_boxplot(outlier.size = ifelse(input$outliers, 2, NA)) +
xlab(input$variable)
print(p)
})
})



my ui.R code is as followed


library(shiny)

# Define UI for miles per gallon application
shinyUI(pageWithSidebar(

  # Application title
  headerPanel("Miles Per Gallon"),

  # Sidebar with controls to select the variable to plot against mpg
  # and to specify whether outliers should be included
  sidebarPanel(
    selectInput("variable", "Variable:",
                list("Cylinders" = "cyl", 
                     "Transmission" = "am", 
                     "Gears" = "gear")),

    checkboxInput("outliers", "Show outliers", FALSE)
  ),

  mainPanel()
))










