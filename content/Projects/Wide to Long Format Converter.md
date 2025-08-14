---
title: "Wide to Long Converter"
output: html_document
runtime: shiny
---

After all that hard work in the lab, you finally have your data… but wait a second… 😱
It’s in the wrong format?! Don’t worry, I got your back!

Use this Wide-to-Long Converter to get your data R-ready in a snap.
Just upload your CSV or Excel file, and it will magically transform it into a long format CSV





```{r, echo=FALSE}
library(shiny)
library(readr)
library(readxl)
library(dplyr)
library(tidyr)

ui <- fluidPage(
  titlePanel("Wide to Long CSV/Excel Converter"),
  sidebarLayout(
    sidebarPanel(
      fileInput("file", "Choose CSV or Excel File", accept = c(".csv", ".xlsx")),
      uiOutput("id_selector"),
      downloadButton("downloadData", "Download Converted File")
    ),
    mainPanel(
      tableOutput("preview")
    )
  )
)

server <- function(input, output, session) {
  
  data <- reactive({
    req(input$file)
    file <- input$file$datapath
    
    if(grepl("\\.csv$", input$file$name)) {
      df <- read_csv(file)
    } else {
      df <- read_excel(file)
    }
    df
  })
  
  output$id_selector <- renderUI({
    req(data())
    selectInput("id_col", "Select ID column(s):", choices = names(data()), multiple = TRUE)
  })
  
  long_data <- reactive({
    req(data(), input$id_col)
    pivot_longer(data(), cols = -all_of(input$id_col), names_to = "variable", values_to = "value")
  })
  
  output$preview <- renderTable({
    req(long_data())
    head(long_data(), 10)  # show first 10 rows
  })
  
  output$downloadData <- downloadHandler(
    filename = function() { "converted_long.csv" },
    content = function(file) {
      write_csv(long_data(), file)
    }
  )
}

shinyApp(ui, server)
