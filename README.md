World Layoffs SQL Data Cleaning & Analysis

This project demonstrates an end-to-end SQL workflow for cleaning, deduplicating, and preparing the World Layoffs dataset for analysis.
It follows a structured data-engineering approach using staging, transformation, and validation steps.

Project Overview
Goal: Transform a raw layoffs dataset into a clean, duplicate-free table ready for analytics and reporting.
Key Outcomes:

Identified and removed duplicate records using SQL window functions.

Standardized text and date fields for consistent formatting.

Produced a verified clean table for KPI generation and dashboard use.



Workflow Steps
1. Create Staging Table

Clone the raw data into a staging table for safe cleaning:
CREATE TABLE world_layoffs.layoffs_staging LIKE world_layoffs.layoffs;
INSERT INTO world_layoffs.layoffs_staging SELECT * FROM world_layoffs.layoffs;

2. Standardize and Clean Fields

Trim spaces, unify capitalization, and handle missing values:
UPDATE world_layoffs.layoffs_staging
SET company = TRIM(company),
    location = TRIM(location),
    industry = NULLIF(industry, '');

3. Remove Duplicates Using ROW_NUMBER()

Rank records by identical attributes, then remove rows where row_num > 1:
CREATE TABLE world_layoffs.layoffs_clean LIKE world_layoffs.layoffs;
INSERT INTO world_layoffs.layoffs_clean
SELECT * FROM world_layoffs.layoffs_staging;

4. Publish Clean Table
Once validated, the clean dataset is published as:
CREATE TABLE world_layoffs.layoffs_clean LIKE world_layoffs.layoffs;
INSERT INTO world_layoffs.layoffs_clean
SELECT * FROM world_layoffs.layoffs_staging;
This table becomes the single source of truth for analytics.

5. Generate KPIs
SELECT YEAR(`date`) AS year, SUM(total_laid_off) AS total_laid_off
FROM world_layoffs.layoffs_clean
GROUP BY YEAR(`date`);


What This Project Demonstrates
Building a reproducible SQL pipeline
Using window functions for intelligent deduplication
Applying data validation checkpoints
Delivering a ready-to-analyze dataset for Power BI/Tableau

Tools & Technologies
Database: MySQL
Language: SQL
Techniques: Data Cleaning, Transformation, Deduplication, KPI Analytics
Functions Used: ROW_NUMBER(), TRIM(), NULLIF(), GROUP BY
