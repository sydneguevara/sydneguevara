---
title: "Wide to Long Converter"
output: html_document
runtime: shiny
date: 2025-010-05
---

After all that hard work in the lab, you finally have your data… but wait a second… 😱  
It’s in the wrong format?! Don’t worry, I got your back!  

Use this Wide-to-Long Converter to get your data R-ready in a snap. Just upload your CSV or Excel file, and it will magically transform it into a long format CSV.

📖 Quick explanation

Wide format: many columns, each subject/observation is in a single row, with different variables spread across many columns.

Long format: many rows, each row is one measurement, with columns that say who/what it belongs to and the value.


Example: 

This:

| ID | Height\_2023 | Height\_2024 |
| -- | ------------ | ------------ |
| 1  | 150          | 160          |
| 2  | 140          | 145          |

Becomes: 

| ID | Year | Height |
| -- | ---- | ------ |
| 1  | 2023 | 150    |
| 1  | 2024 | 160    |
| 2  | 2023 | 140    |
| 2  | 2024 | 145    |


[Launch the App 🚀](https://sydne.shinyapps.io/Wide_to_Long_Converter/)