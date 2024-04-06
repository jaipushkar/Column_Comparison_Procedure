# Column_Comparison_Procedure

Column Comparison Procedure
This repository contains a SQL stored procedure for comparing the columns of a specified table against an expected sequence.

Overview
The provided SQL stored procedure QA_COMPARE_COLUMNS compares the columns of a specified table (@TableName) against an expected sequence of column names. It checks whether the actual column sequence matches the provided sequence.

Stored Procedure
_**CREATE PROCEDURE QA_COMPARE_COLUMNS
(
    @TableName NVARCHAR(255)
)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @sql = '
        SELECT
            CASE
                WHEN (
                    SELECT STRING_AGG(COLUMN_NAME, '','') WITHIN GROUP (ORDER BY ORDINAL_POSITION)
                    FROM INFORMATION_SCHEMA.COLUMNS
                    WHERE TABLE_NAME = ''' + QUOTENAME(@TableName) + '''
                    AND ORDINAL_POSITION BETWEEN 1 AND 30
                ) = ''ID,SITE,ZIP,DATE,IMAGELINK,PRICE,PRICE_MULT,EPRICE,EINDICATOR,ALTPRICE,ALTPRICEMULT,EALTPRICE,ALTINDICATOR,PACK,UNIT,UOM,SIZE,PRODUCT_ID,UPC,ASIN,CATEGORY,DESCRIPTION,NOTE,RATING,VALID,PAGEIMAGE,AISLE,ORIG_UPC,JOB_NUMBER,SALE_ENDDATE''
                THEN ''Column_Name_Correct''
                ELSE ''Column_Name_Incorrect''
            END AS ColumnSequenceMatch';
    EXEC sp_executesql @sql;
END;**_


Description
This stored procedure enables users to verify whether the columns of a given table match an expected sequence. It retrieves the column names from the specified table and compares them against the predefined sequence. If the actual sequence matches the expected sequence, it returns a result indicating that the column names are correct; otherwise, it indicates that the column names are incorrect.

How to Use
Clone this repository to your local machine.
Execute the provided SQL stored procedure QA_COMPARE_COLUMNS in your SQL Server environment.
Provide the required parameter:
@TableName: Name of the table whose columns you want to compare against the expected sequence.
Review the output to determine whether the column sequence matches the expected sequence.
