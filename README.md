# shinywidgets
This package includes some commonly used Shiny customized UI widgets such as a file uploader with data previewing capability.

# Installation
`devtools::install_github("keqiang/shinywidgets")`

# Example: app.R

## Regular Shiny app
```R
library(shiny)
library(shinywidgets)

ui <- fluidPage(
  wellPanel(
    dataTableImportWidget("tableImport1"),
    verbatimTextOutput("debug")
  )
)

server <- function(input, output) {
  importedData <- importDataTable("tableImport1")
  
  output$debug <- renderPrint({
    print(importedData())
  })
}

shinyApp(ui = ui, server = server)

```

## Dashboard Shiny app
```R
library(shiny)
library(shinydashboard)
library(shinywidgets)

ui <- dashboardPage(
  dashboardHeader(title = "Table Importing Example"),
  dashboardSidebar(),
  dashboardBody(
    fluidRow(
      box(title = "Data Import",
          status = "primary",
          solidHeader = TRUE,
          width = 12,
          dataTableImportWidget("tableImport1"),
          verbatimTextOutput("debug")
      )
    )
  )
)

server <- function(input, output) {
  importedData <- importDataTable("tableImport1")
  
  output$debug <- renderPrint({
    print(importedData())
  })
}

shinyApp(ui = ui, server = server)
```

## Import data from server
```R
library(shiny)
library(shinywidgets)

ui <- fluidPage(
  wellPanel(
    dataTableImportWidget("tableImport1"),
    verbatimTextOutput("debug")
  )
)

server <- function(input, output) {
  # specifying fileLocation as "server" or "both" to provide users the option to import a data table from server file
  importedData <- importDataTable("tableImport1", fileLocation = "both")
  
  output$debug <- renderPrint({
    print(importedData())
  })
}

shinyApp(ui = ui, server = server)
```

## Usage within Shiny Module
```R
library(shiny)
library(shinywidgets)

testTableImportInModuleUI <- function(id) {
  ns <- NS(id)
  tagList(
    dataTableImportWidget(ns("tableImport1")),
    verbatimTextOutput(ns("debug"))
  )
}

testTableImportInModule <- function(input, output, session) {
  importedData <- importDataTable("tableImport1")
  
  output$debug <- renderPrint({
    print(importedData())
  })
}


ui <- fluidPage(
  wellPanel(
    testTableImportInModuleUI("test")
  )
)

server <- function(input, output) {
  callModule(testTableImportInModule, "test")
}

shinyApp(ui = ui, server = server)
```
