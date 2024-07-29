# World-Layoffs-EDA

## Project Overview
The main objective of this project is to conduct an in-depth exploratory data analysis of a dataset on world layoffs. By leveraging MySQL's powerful querying capabilities, the project delves into various aspects of the layoff data to uncover meaningful patterns and trends.

## Objectives
- Understand Layoff Patterns: Explore the dataset to identify patterns and trends in layoffs across different industries and regions.
- Analyze Temporal Trends: Investigate how layoffs have changed over time and identify any significant periods of increased activity.
- Examine Industry Impacts: Analyze which industries have been most affected by layoffs and explore potential underlying reasons.
- Discover Geographical Differences: Understand how layoffs vary across different geographical regions and identify potential causes.
- Provide Insights for Decision-Making: Use the findings to offer insights that can inform business strategies and policy decisions.

  ## Tools and Techniques
  1. MySQL: Utilized MySQL for data exploration, querying, and analysis, enabling a thorough examination of the dataset
  2. SQL Queries: Applied various SQL queries to aggregate, filter, and visualize data, aiding in identifying key patterns.
 
  
 
```sql
-- Exploratory Data Analytics
  
  SELECT *
  FROM layoffs_staging3;
  
   SELECT MAX(total_laid_off), MAX(percentage_laid_off)
  FROM layoffs_staging3; 
  
  SELECT *
  FROM layoffs_staging3
  WHERE percentage_laid_off = 1
  ORDER BY total_laid_off DESC;
  
   SELECT company, SUM(total_laid_off)
  FROM layoffs_staging3
  GROUP BY company
  ORDER BY 2 DESC;
  
  SELECT MIN(`date`), MAX(`date`)
  from layoffs_staging3;
  
   SELECT industry, SUM(total_laid_off)
  FROM layoffs_staging3
  GROUP BY industry
  ORDER BY 2 DESC;
  
 SELECT YEAR(`date`), SUM(total_laid_off)
 FROM layoffs_staging3
 GROUP BY YEAR(`date`)
 ORDER BY 1 DESC;
 
  SELECT stage, SUM(total_laid_off)
  FROM layoffs_staging3
  GROUP BY stage
  ORDER BY 2 DESC;
  
   SELECT SUBSTRING(`date`, 1, 7) AS `MONTH`, SUM(total_laid_off)
   FROM layoffs_staging3
   WHERE SUBSTRING(`date`, 1, 7) IS NOT NULL
   GROUP BY `MONTH`
   ORDER BY 1 ASC;
   
WITH Rolling_Total AS
(
SELECT SUBSTRING(`date`, 1, 7) AS `MONTH`, SUM(total_laid_off) AS Total_off
   FROM layoffs_staging3
   WHERE SUBSTRING(`date`, 1, 7) IS NOT NULL
   GROUP BY `MONTH`
   ORDER BY 1 ASC
   )
   SELECT `MONTH`, total_off,
   SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total
   FROM Rolling_Total;
   
   SELECT company, YEAR(`date`), SUM(total_laid_off)
   FROM layoffs_staging3
   GROUP BY company, YEAR(`date`)
   ORDER BY 3 DESC;
   
   WITH Company_year (company, years, total_laid_off) AS
   (
   SELECT company, YEAR(`date`), SUM(total_laid_off)
   FROM layoffs_staging3
   GROUP BY company, YEAR(`date`)
   ), Company_Year_Rank AS 
   (SELECT *, DENSE_RANK() OVER (PARTITION BY years ORDER BY total_laid_off DESC) AS Ranking
   FROM Company_Year
   WHERE yearS IS NOT NULL
   )
   SELECT *
   FROM Company_Year_Rank
   WHERE Ranking <= 5;
```

## Conclusion
This project highlights the power of exploratory data analysis in uncovering critical insights into global employment trends. By exploring layoff data with MySQL, I gained a deeper understanding of the factors driving layoffs and the potential impacts on economies worldwide. The insights derived from this project can aid in strategic decision-making and contribute to more informed discussions on employment and economic policy.


   
   
