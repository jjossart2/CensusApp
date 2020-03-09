    #install.packages(c("maps", "mapproj"))
    library(maps)
    library(mapproj)
    source("helpers.R")
    counties <- readRDS("data/counties.rds")
    percent_map(counties$white, "darkgreen", "% White")

![](README_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    library(shiny)
    library(maps)
    library(mapproj)

    counties <- readRDS("data/counties.rds")
    source("helpers.R")
    ui <- fluidPage(
      titlePanel("censusVis"),
      
      sidebarLayout(
        sidebarPanel(
          helpText("Create demographic maps with 
            information from the 2010 US Census."),
          
          selectInput("var", 
                      label = "Choose a variable to display",
                      choices = c("Percent White", "Percent Black",
                                  "Percent Hispanic", "Percent Asian"),
                      selected = "Percent White"),
          
          sliderInput("range", 
                      label = "Range of interest:",
                      min = 0, max = 100, value = c(0, 100))
        ),
        
        mainPanel(plotOutput("map"))
      )
    )

    server <- function(input, output) {
      output$map <- renderPlot({
        data <- switch(input$var, 
                       "Percent White" = counties$white,
                       "Percent Black" = counties$black,
                       "Percent Hispanic" = counties$hispanic,
                       "Percent Asian" = counties$asian)
        
        color <- switch(input$var, 
                        "Percent White" = "darkgreen",
                        "Percent Black" = "black",
                        "Percent Hispanic" = "darkorange",
                        "Percent Asian" = "darkviolet")
        
        legend <- switch(input$var, 
                         "Percent White" = "% White",
                         "Percent Black" = "% Black",
                         "Percent Hispanic" = "% Hispanic",
                         "Percent Asian" = "% Asian")
        
        percent_map(data, color, legend, input$range[1], input$range[2])
      })
    }

    shinyApp(ui, server)

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

<!--html_preserve-->
Shiny applications not supported in static R Markdown documents

<!--/html_preserve-->
